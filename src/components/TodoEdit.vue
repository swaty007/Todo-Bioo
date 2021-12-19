<template>
    <v-container v-if="todo">
      <v-row class="text-center">
        <v-col cols="12">
          <v-img
            :src="require('../assets/logo.svg')"
            class="my-3"
            contain
            height="200"
          />
          <v-text-field
            label="Title"
            ref="todo"
            :rules="rulesTodo"
            hide-details="auto"
            v-model="todo.text"
            @keyup.enter="addTodo"
          ></v-text-field>
          <v-checkbox
            label="Completed"
            v-model="todo.completed"
          ></v-checkbox>
          <v-btn
            class="ma-2"
            color="success"
            @click="save"
          >
            Save
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
</template>

<script>

export default {
  name: 'TodoEdit',
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  data: () => ({
    rulesTodo: [
      value => !!value || 'Required.',
      value => (value && value.length >= 3) || 'Min 3 characters',
    ],
    newTodo: '',
    todos: JSON.parse(localStorage.getItem('todos') || '[]'),
    todo: null,
  }),
  created () {
    this.todo = this.todos.find(t => t.id === this.id)
  },
  methods: {
    save () {
      localStorage.setItem('todos', JSON.stringify(this.todos))
      this.$router.push({ name: 'home' })
    },
  },
}
</script>
