<template>
  <header>
    <div class="header">
      <img :src="backgroundImage" />

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
      },
      backgroundImage() {
        return this.darkTheme ?
          require('../assets/bg-desktop-dark.jpg' ) :
          require('../assets/bg-desktop-light.jpg');
      },
    }
  }
</script>

<style scoped>
  header {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    width: 100%;
    height: 250px;
    background-color: #1F1F1F;
  }

  .dark header {
    background-color: #0B0B0C;
  }

  .header {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: calc(100% - 48px);
    max-width: 500px;
    height: 100%;
    margin: 0 24px;
  }

  .header img {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    object-fit: cover;
  }

  .logo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    margin-bottom: 25px;
    z-index: 2;
  }

  h1 {
    color: #FFFFFF;
    font-size: 1.8rem;
    font-weight: 700;
    letter-spacing: 7px;
  }

  button { cursor: pointer; }

  button i { color: #FFFFFF; font-size: 1.15rem; }
</style>
