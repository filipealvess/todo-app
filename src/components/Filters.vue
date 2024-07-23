<template>
  <div class="filters">
    <p>{{ completedTodosQuantity }}/{{ todosQuantity }}</p>

    <button type="button" class="btn-filter" @click="showFilters">
      <i class="fas fa-filter"></i>
      Filtrar
    </button>

    <div class="filter-buttons">
      <button
        type="button"
        class="selected"
        data-filter="all"
        @click="changeFilter"
      >
        Tudo
      </button>

      <button type="button" data-filter="active" @click="changeFilter">
        Ativos
      </button>

      <button type="button" data-filter="completed" @click="changeFilter">
        Completos
      </button>

      <button type="button" class="hide-filters" @click="hideFilters">
        <i class="fas fa-times"></i>
      </button>
    </div>

    <button
      type="button"
      class="clear"
      @click="$emit('clear-completed-todos')"
    >Limpar Completos</button>
  </div>
</template>

<script>
  export default {
    name: 'Filters',
    props: {
      completedTodosQuantity: Number,
      todosQuantity: Number
    },
    methods: {
      changeFilter({ target }) {
        const filterButtons = document.querySelectorAll('[data-filter]');
        const filterType = target.dataset.filter;

        filterButtons.forEach(button => button.classList.remove('selected'));
        target.classList.add('selected');
        this.$emit('filter-todos', filterType);
      },
      showFilters() {
        const filtersContainer = document.querySelector('.filter-buttons');

        filtersContainer.classList.add('visible');
      },
      hideFilters() {
        const filtersContainer = document.querySelector('.filter-buttons');

        filtersContainer.classList.remove('visible');
      }
    }
  }
</script>

<style scoped>
  .filters {
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: sticky;
    top: 0;
    padding: 5px 20px;
    background-color: var(--bg-items);
    box-shadow: 0 0 2px 1px var(--text-secondary-color);
  }

  .filters * {
    color: var(--text-secondary-color);
    font-size: 0.7rem;
    font-weight: 700;
    transition: 0.2s color;
  }

  button:not(.selected):hover, button:hover i {
    color: var(--text-primary-color);
  }

  button { padding: 10px 0; cursor: pointer; }

  .filter-buttons button { padding: 10px 7px; }

  .selected { color: var(--blue); }

  .hide-filters {
    display: none;
    position: absolute;
    right: 20px;
  }

  .btn-filter {
    display: none;
    padding: 10px;
  }

  @media (max-width: 450px) {
    .filter-buttons {
      display: flex;
      justify-content: center;
      align-items: center;
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: var(--bg-items);
      transform: translateY(-100%);
      transition: 0.2s transform;
    }

    .filter-buttons.visible { transform: translateY(0); }

    .filter-buttons button:nth-child(3) { margin-right: 50px; }

    .hide-filters { display: block; }

    .btn-filter { display: block; }

    button:not(.selected):hover, button:hover i {
      color: var(--text-secondary-color);
    }
  }
</style>
