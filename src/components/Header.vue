<template>
  <header>
    <div class="header">
      <div class="logo">
        <h1>TODO</h1>
        <button type="button" @click="$emit('toggle-theme')">
          <i class="fas" :class="iconClass"></i>
        </button>
      </div>

      <AddTodo @add-todo="handleNewTodo" />
    </div>
  </header>
</template>

<script>
  import AddTodo from './AddTodo.vue';

  export default {
    name: 'Header',
    components: {
      AddTodo
    },
    props: {
      darkTheme: Boolean
    },
    methods: {
      handleNewTodo(newTodo) {
        this.$emit('add-todo', newTodo);
      }
    },
    computed: {
      iconClass() {
        return this.darkTheme ? 'fa-sun' : 'fa-moon';
      }
    }
  }
</script>

<style scoped>
  header {
    height: 35vh;
    min-height: 200px;
    background: url('../assets/bg-desktop-light.jpg') left top no-repeat;
  }

  .dark header {
    background: url('../assets/bg-desktop-dark.jpg') left top no-repeat;
  }

  .header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 80%;
    max-width: 500px;
    height: 100%;
    margin: 0 auto;
  }

  .logo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    margin-bottom: 25px;
  }

  h1 {
    color: #FFFFFF;
    font-size: 1.8rem;
    font-weight: 700;
    letter-spacing: 7px;
  }

  button { cursor: pointer; }

  button i { color: #FFFFFF; font-size: 1.15rem; }

  @media (max-width: 350px) {
    header {
      background: url('../assets/bg-mobile-light.jpg') left top no-repeat;
    }

    .dark header {
      background: url('../assets/bg-mobile-dark.jpg') left top no-repeat;
    }
  }
</style>
