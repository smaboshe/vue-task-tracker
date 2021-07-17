<template>
  <header>
    <h1>{{ title }}</h1>
    <Button :text="showAddTask ? 'Close' : 'Add Task'" :color="showAddTask ? 'black' : 'green'" @button-click="$emit('toggle-add-task')" v-show="isHomePage" />
  </header>
</template>

<script>
  import Button from './Button'

  export default {
    name: 'Header',
    props: {
      title: String,
      showAddTask: Boolean
    },
    components: {
      Button
    },
    computed: {
      isHomePage() {
        if(this.$route.path ===  '/') {
          return true
        } else {
          return false
        }
      }
    }
  }
</script>

<style scoped>
  header  {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>
