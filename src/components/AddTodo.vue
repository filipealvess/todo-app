<template>
  <div class="new-todo">
    <input
      type="text"
      placeholder="Novo Todo..."
      v-model="newTodo"
      @keydown.enter="handleNewTodo"
    >

    <button type="button" @click="handleNewTodo">
      <i class="fas fa-plus-circle"></i>
    </button>
  </div>
</template>

<script>
  export default {
    name: 'AddTodo',
    data() {
      return {
        newTodo: ''
      }
    },
    methods: {
      handleNewTodo() {
        const isValidTodo = this.newTodo.trim().length > 0;

        if (isValidTodo) {
          this.$emit('add-todo', this.newTodo);
          this.newTodo = '';
        }
      }
    }
  }
</script>

<style scoped>
  .new-todo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    padding: 10px 15px;
    background-color: var(--bg-items);
    border-radius: 3px;
  }

  input {
    flex-basis: 100%;
    width: 80%;
    padding: 5px;
    color: var(--text-primary-color);
  }

  button { display: flex; cursor: pointer; }

  i {
    color: var(--text-secondary-color);
    transition: 0.2s color;
    font-size: 1.15rem;
  }

  button:hover i { color: var(--text-primary-color); }

  @media (max-width: 400px) {
    button:hover i { color: var(--text-secondary-color); }
  }
</style>
