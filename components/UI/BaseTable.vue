<template>
  <table class="titles">
    <thead>
    <tr>
      <th>List of Titles</th>
    </tr>
    </thead>

    <tbody>
    <tr v-for="{ title, id } in posts" :key="id">
      <td>{{ title }}</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "BaseTable",
  props: {
    posts: {
      type: Array,
      default: [],
    }
  }
}
</script>

<style lang="scss">
.titles {
  border-collapse: collapse;
  width: 100%;

  th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: $main-color;
    color: $white-color;
  }

  tr {
    transition: all 0.5s ease;
  }

  td, th {
    border: 1px solid $dark-gray;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: $light-gray;
  }

  tr:hover {
    background-color: $dark-gray;
  }
}
</style>