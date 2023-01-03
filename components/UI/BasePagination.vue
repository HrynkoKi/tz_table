<template>
  <div class="pagination">
    <div
      v-for="pageNumber in pages"
      :key="pageNumber"
      @click="$emit('change', pageNumber)"
      :class="['pagination__item', { 'pagination__selected': pageNumber === selectedPage }]"
    >
      {{ pageNumber }}
    </div>
  </div>
</template>

<script>
export default {
  name: "BasePagination",
  emits: ['change'],
  props: {
    pages: {
      type: Number,
      default: 0,
    },
    selectedPage: {
      type: Number,
      default: 1,
    }
  }
}
</script>

<style lang="scss">
.pagination {
  display: flex;
  align-items: center;
  gap: 20px;

  &__item {
    display: inline-block;
    cursor: pointer;
  }

  &__selected {
    border: 1px solid $main-color;
    padding: 10px;
  }
}
</style>