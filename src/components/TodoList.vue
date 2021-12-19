<template>
  <div>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/logo.svg')"
          class="my-3"
          contain
          height="200"
        />
        <v-text-field
          label="What need to be done?"
          ref="todo"
          :rules="rulesTodo"
          hide-details="auto"
          v-model="newTodo"
          @keyup.enter="addTodo"
        ></v-text-field>
      </v-col>
      <v-col>
        <v-list-item-title v-if="todos.length" class="text-left" v-text="`Total: ${todos.length}`"></v-list-item-title>
        <v-list>
          <TodoItem
            v-for="todo in todos"
            :key="todo.id"
            :item="todo"
            @toggle-status="toggleStatus"
            @remove="removeTodo"
          />
        </v-list>
      </v-col>
    </v-row>
    <v-btn
      class="ma-2"
      color="success"
      @click="saveJson"
    >
      Save
    </v-btn>

    <v-file-input
      accept="application/json"
      placeholder="Load Todo File"
      label="Load Todo"
      @change="loadJson"
    ></v-file-input>
  </v-container>
  <confirm-dialogue ref="confirmDialogue"></confirm-dialogue>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'
import ConfirmDialogue from './ConfirmDialogue.vue'

export default {
  name: 'TodoList',
  components: {
    TodoItem,
    ConfirmDialogue,
  },
  data: () => ({
    rulesTodo: [
      value => !!value || 'Required.',
      value => (value && value.length >= 3) || 'Min 3 characters',
    ],
    newTodo: '',
    todos: JSON.parse(localStorage.getItem('todos') || '[]'),
    todosIndex: 0,
  }),
  created () {
    for (const todo of this.todos) {
      if (todo.id > this.todosIndex) {
        this.todosIndex = todo.id
      }
    }
  },
  watch: {
    todos: {
      handler: (todos) => {
        localStorage.setItem('todos', JSON.stringify(todos))
      },
      deep: true,
    },
  },
  methods: {
    addTodo () {
      if (this.$refs.todo.validate() && this.newTodo) {
        this.todos.push({
          id: ++this.todosIndex,
          text: this.newTodo,
          completed: false,
        })
        this.newTodo = ''
      }
    },
    removeTodo (id) {
      const index = this.todos.findIndex(i => i.id === id)
      this.$refs.confirmDialogue.show({
        title: 'Delete Todo',
        message: 'Are you sure you want to delete todo? It cannot be undone.',
        okButton: 'Delete Todo',
      }).then((result) => {
        if (result) {
          this.todos.splice(index, 1)
        }
      })
    },
    toggleStatus (id) {
      const item = this.todos.find(i => i.id === id)
      item.completed = !item.completed
    },
    saveJson () {
      const bl = new Blob([JSON.stringify(this.todos)], {
        type: 'text/html',
      })

      // Internet Explorer Support
      const a = document.createElement('a')
      a.href = URL.createObjectURL(bl)
      a.download = 'todos.json'
      a.hidden = true
      document.body.appendChild(a)
      a.innerHTML =
        'someinnerhtml'
      a.click()
    },
    loadJson (event) {
      if (event) {
        const reader = new FileReader()
        reader.onload = (event) => {
          const data = JSON.parse(event.target.result)
          if (data) {
            this.$refs.confirmDialogue.show({
              title: 'Load Todo From File',
              message: 'Are you sure you want to load data from JSON? It cannot be undone.',
              okButton: 'Load Todo',
            }).then((result) => {
              if (result) {
                this.todos = data
              }
            })
          }
        }
        reader.readAsText(event)
      }
    },
  },
}
</script>
