<template>
  <div class="todo" v-if="todo.visible">
    <button
      type="button"
      class="complete"
      @click="$emit('complete-todo', todo.id)"
    >
      <i :class="iconClasses"></i>
    </button>

    <p
      :class="{ 'is-completed' : todo.completed }"
      @blur="disableContentEditing"
      @keydown.enter="disableContentEditing"
    >
      {{ todo.title }}
    </p>

    <div class="options">
      <button type="button" class="options-button" @click="toggleOptions">
        <i class="fas fa-ellipsis-v"></i>
      </button>

      <Dropdown
        v-if="optionsVisible"
        :options="[
          {
            id: 'update',
            onClick: () => null,
            text: 'Editar',
          },
          {
            id: 'delete',
            onClick: () => null,
            text: 'Excluir',
          },
        ]"
      />
    </div>
  </div>
</template>

<script>
  import Dropdown from './Dropdown.vue';

  export default {
    name: 'Todo',
    components: {
      Dropdown,
    },
    props: {
      todo: Object
    },
    data() {
      return {
        optionsVisible: false,
      }
    },
    methods: {
      enableContentEditing({ target }) {
        target.contentEditable = true;
        target.focus();
      },
      disableContentEditing({ target }) {
        target.contentEditable = false;

        this.$emit('update-todo', this.todo.id, target.textContent.trim());
      },
      toggleOptions() {
        this.optionsVisible = !this.optionsVisible;
      },
    },
    computed: {
      iconClasses() {
        const circle = ['far', 'fa-circle'];
        const checkCircle = ['fas', 'fa-check-circle'];

        return this.todo.completed ? checkCircle : circle;
      }
    }
  }
</script>

<style scoped>
  .todo {
    display: flex;
    align-items: center;
    width: 100%;
    padding: 15px 20px;
    background-color: var(--bg-items);
    border-top: 1px solid var(--text-secondary-color);
    cursor: move;
  }

  * { transition: 0.2s color; }

  p {
    flex-basis: 100%;
    max-width: 85%;
    padding-left: 5px;
    margin: 0 15px 0 10px;
    border-bottom: 1px solid transparent;
    color: var(--text-primary-color);
    overflow-wrap: break-word;
    word-wrap: break-word;
    transition: 0.2s border, color;
  }

  p:focus {
    border-bottom-color: var(--blue);
  }

  .is-completed {
    color: var(--text-secondary-color);
    text-decoration: line-through;
  }

  button { cursor: pointer; }

  button i {
    font-size: 1.15rem;
    color: var(--text-secondary-color);
  }

  button:hover i:not(.fa-check-circle) {
    color: var(--text-primary-color);
  }

  .fa-check-circle { color: var(--blue); }

  .options {
    display: flex;
    position: relative;
    padding: 0;
    margin: 0;
  }

  .options-button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 24px;
    height: 24px;
  }

  .options-button * {
    font-size: 16px;
  }
</style>
