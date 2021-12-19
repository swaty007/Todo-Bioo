<template>
  <!--  <hello-world />-->
  <TodoEdit :id="Number(this.$route.params.id)"/>
</template>

<script>
// import HelloWorld from '../components/HelloWorld'
import TodoEdit from '../../components/TodoEdit'

export default {
  name: 'edit',

  components: {
    // HelloWorld,
    TodoEdit,
  },
}
</script>
