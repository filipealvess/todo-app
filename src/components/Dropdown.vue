<template>
  <section>
    <button
      v-for="option in options"
      :key="option.id"
      @click="option.onClick"
    >
      {{ option.text }}
    </button>
  </section>
</template>

<script>
  export default {
    name: 'Dropdown',
    props: {
      options: Array,
    },
  }
</script>

<style scoped>
  section {
    display: flex;
    flex-direction: column;
    position: absolute;
    top: 130%;
    right: 0;
    min-width: 100px;
    border-radius: 5px;
    background-color: var(--bg-items);
    box-shadow: 0 5px 15px 2px rgb(139 126 244 / 30%);
    z-index: 10;
  }

  button {
    padding: 12px;
    color: var(--text-secondary-color);
    font-size: 0.7rem;
    font-weight: 700;
    text-align: left;
    cursor: pointer;
    transition: 0.3s color;
  }

  button:hover {
    color: var(--text-primary-color);
  }
</style>
