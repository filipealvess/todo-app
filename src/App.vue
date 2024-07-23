<template>
  <div class="app" :class="{ 'dark' : darkTheme }">
    <Header
      :darkTheme="darkTheme"
      @toggle-theme="() => { this.darkTheme = !this.darkTheme }"
      @add-todo="addNewTodo"
    />

    <Todos
      :todos="completedTodosAreDown"
      @complete-todo="completeTodo"
      @remove-todo="removeTodo"
      @update-todo="updateTodo"
      @filter-todos="filterTodos"
    />
  </div>
</template>

<script>
  import Header from './components/Header.vue';
  import Todos from './components/Todos.vue';

  export default {
    name: 'App',
    components: {
      Header,
      Todos
    },
    data() {
      return {
        darkTheme: false,
        id: 0,
        todos: []
      }
    },
    methods: {
      addNewTodo(title) {
        const newTodo = { id: this.id, title, completed: false, visible: true };

        this.id++;
        this.todos.push(newTodo);
      },
      completeTodo(todoId) {
        const targetTodo = this.todos.filter(todo => todo.id === todoId)[0];

        targetTodo.completed = !targetTodo.completed;
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      removeTodo(todoId) {
        const targetTodo = this.todos.filter(todo => todo.id === todoId)[0];
        const index = this.todos.indexOf(targetTodo);

        this.todos.splice(index, 1);
      },
      updateTodo(todoId, todoTitle) {
        const targetTodo = this.todos.filter(todo => todo.id === todoId)[0];

        targetTodo.title = todoTitle;
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      filterTodos(filteredTodos) {
        this.todos.forEach(todo => ( todo.visible = false ));

        this.todos.forEach(todo => {
          filteredTodos.forEach(filteredTodo => {
            if (todo.id === filteredTodo.id) {
              todo.visible = true;
            }
          });
        });
      }
    },
    computed: {
      completedTodosAreDown() {
        const uncompletedTodos = this.todos.filter(todo => !todo.completed);
        const completedTodos = this.todos.filter(todo => todo.completed);

        return [...uncompletedTodos, ...completedTodos];
      }
    },
    watch: {
      darkTheme() {
        localStorage.setItem('darkTheme', this.darkTheme);
      },
      todos() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      }
    },
    created() {
      const darkThemeValue = JSON.parse(localStorage.getItem('darkTheme'));
      const savedTodos = JSON.parse(localStorage.getItem('todos'));
      const thereAreSavedTodos = savedTodos != null && savedTodos.length;

      if (darkThemeValue != null) this.darkTheme = darkThemeValue;

      if (thereAreSavedTodos) {
        const lastTodo = savedTodos[savedTodos.length - 1];

        this.todos = savedTodos;
        this.id = lastTodo.id + 1;
      }
    }
  }
</script>

<style>
  html { font-size: 18px; }

  body {
    overflow-x: hidden;
  }

  .app {
    --bg: #FAFAFA;
    --bg-items: #FFFFFF;
    --blue: #8B7EF4;
    --text-primary-color: #616071;
    --text-secondary-color: #9E9DA3;

    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100vw;
    min-height: 100vh;
    background-color: var(--bg);
  }

  .app.dark {
    --bg: #181824;
    --bg-items: #25273C;
    --text-primary-color: #B2B4CB;
    --text-secondary-color: #5B5D76;
  }

  * {
    padding: 0;
    margin: 0;
    border: 0;
    background: transparent;
    font-family: 'Josefin Sans', sans-serif;
    font-size: 1rem;
    font-weight: 400;
    box-sizing: border-box;
    outline: 0;
    transition: 0.4s background-color, color;
  }
</style>
