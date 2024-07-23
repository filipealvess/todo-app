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

    <button type="button" class="remove" @click="$emit('remove-todo')">
      <i class="fas fa-times"></i>
    </button>
  </div>
</template>

<script>
  export default {
    name: 'Todo',
    props: {
      todo: Object
    },
    methods: {
      enableContentEditing({ target }) {
        target.contentEditable = true;
        target.focus();
      },
      disableContentEditing({ target }) {
        target.contentEditable = false;

        this.$emit('update-todo', this.todo.id, target.textContent.trim());
      }
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

  .todo:hover .remove { opacity: 1; }

  .remove { opacity: 0; transition: 0.2s opacity, color; }

  @media (max-width: 600px) {
    p { max-width: 80%; }
  }

  @media (max-width: 400px) {
    p { max-width: 70%; }

    .remove { opacity: 1; }

    button:hover i:not(.fa-check-circle) {
      color: var(--text-secondary-color);
    }
  }
</style>
