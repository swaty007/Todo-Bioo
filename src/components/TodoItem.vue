<template>
  <div>
    <v-list-item :class="{'grey':item.completed}">
      <v-list-item-action>
        <v-checkbox
          :input-value="item.completed"
          @change="$emit('toggle-status', item.id)"
        ></v-checkbox>
      </v-list-item-action>
      <v-list-item-content :class="{'v-list-item--done':item.completed}">
        <v-list-item-title v-text="item.text"></v-list-item-title>
      </v-list-item-content>

      <v-list-item-action>
        <v-icon
          @click="$emit('remove', item.id)"
          color="red lighten-1">
          mdi-delete
        </v-icon>
        <router-link :to="{ name: 'todo.edit', params: { id: item.id } }">
          <v-icon
            color="green lighten-1">
            mdi-pencil
          </v-icon>
        </router-link>
      </v-list-item-action>

    </v-list-item>
    <v-divider
    ></v-divider>
  </div>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data: () => ({
    rulesTodo: [
      value => !!value || 'Required.',
      value => (value && value.length >= 3) || 'Min 3 characters',
    ],
    newTodo: '',
  }),
  methods: {
    addTodo (value) {
      console.log(value)
      console.log(this.$refs.todo.validate())
      if (this.$refs.todo.validate()) {
        console.log(this.newTodo)
      }
    },
  },
}
</script>

<style scoped lang="scss">
  .v-list-item--done {
    text-decoration: line-through;
  }
</style>
