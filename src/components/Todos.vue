<template>
  <div class="todo-list">
    <Filters
      :completedTodosQuantity="completedTodosQuantity"
      :todosQuantity="todos.length"
      @filter-todos="filterTodos"
      @clear-completed-todos="clearCompletedTodos"
    />

    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @remove-todo="$emit('remove-todo', todo.id)"
      @complete-todo="completeTodo"
      @update-todo="updateTodo"
    />

    <div class="empty" v-if="noTodos">
      <p>Sua lista está vazia.</p>
      <p>Use o campo acima para adicionar um novo Todo.</p>
    </div>

    <div class="empty" v-if="noActiveTodos">
      <p>Você não possui Todos ativos.</p>
    </div>

    <div class="empty" v-if="noCompletedTodos">
      <p>Você não possui Todos completos.</p>
    </div>
  </div>
</template>

<script>
  import TodoItem from './TodoItem.vue';
  import Filters from './Filters.vue';

  export default {
    name: 'Todos',
    components: {
      TodoItem,
      Filters
    },
    props: {
      todos: Array,
    },
    data() {
      return {
        activeTab: 'all'
      }
    },
    methods: {
      filterTodos(filter) {
        const filters = {
          all: this.todos,
          active: this.todos.filter(todo => !todo.completed),
          completed: this.todos.filter(todo => todo.completed)
        }

        this.$emit('filter-todos', filters[filter]);
        this.activeTab = filter;
      },
      clearCompletedTodos() {
        this.todos.forEach(todo => {
          if (todo.completed) this.$emit('remove-todo', todo.id);
        });
      },
      completeTodo(todoId) {
        this.$emit('complete-todo', todoId);
        this.filterTodos(this.activeTab);
      },
      updateTodo(todoId, todoTitle) {
        this.$emit('update-todo', todoId, todoTitle);
      }
    },
    computed: {
      completedTodosQuantity() {
        return this.todos.reduce((quantity, todo) => {
          return todo.completed ? quantity + 1 : quantity;
        }, 0);
      },
      noTodos() {
        const tabIsCorrect = (this.activeTab === 'all');
        const noTodos = (this.todos.length === 0);

        return tabIsCorrect && noTodos;
      },
      noActiveTodos() {
        const tabIsCorrect = (this.activeTab === 'active');
        const noActiveTodos = this.todos.reduce((exists, todo) => {
          return todo.completed ? exists : false;
        }, true);

        return tabIsCorrect && noActiveTodos;
      },
      noCompletedTodos() {
        const tabIsCorrect = (this.activeTab === 'completed');
        const noCompletedTodos = this.todos.reduce((exists, todo) => {
          return todo.completed ? false : exists;
        }, true);

        return tabIsCorrect && noCompletedTodos;
      }
    },
    watch: {
      todos() {
        this.filterTodos(this.activeTab);
      }
    }
  }
</script>

<style scoped>
  .todo-list {
    display: flex;
    flex-direction: column;
    width: calc(100% - 48px);
    max-width: 500px;
    max-height: 500px;
    flex: 1;
    margin: 0 24px;
    border-radius: 3px;
    background-color: var(--bg-items);
    box-shadow: 0 5px 15px 2px rgb(139 126 244 / 30%);
    overflow: auto;
    transform: translateY(-40px);

    /* Barra de Rolagem: Firefox */
    scrollbar-width: thin;
    scrollbar-color: var(--text-secondary-color) transparent;
  }

  .dark .todo-list {
    box-shadow: 0 5px 15px 2px rgb(139 126 244 / 5%);
  }

  /* Barra de Rolagem: Chrome, Edge, Safari */
  .todo-list::-webkit-scrollbar { width: 7px; }

  .todo-list::-webkit-scrollbar-track { background: transparent; }

  .todo-list::-webkit-scrollbar-thumb {
    background-color: var(--text-secondary-color);
    border-radius: 20px;
  }

  .empty {
    display: flex;
    flex-basis: 100%;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    border-top: 1px solid var(--text-secondary-color);
  }

  .empty * { color: var(--text-primary-color); text-align: center; }
</style>
