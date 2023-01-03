<template>
  <div class="table-app">
    <template v-if="$fetchState.pending">
      <div>
        <p>Loading...</p>
      </div>
    </template>

    <template v-else>
      <base-input
        v-model="searchQuery"
        label="Search by Title"
        class="table-app__input"
      />

      <template v-if="todosLoaded">
        <template v-if="filteredPosts.length">
          <base-table
            :posts="filteredPosts"
            class="table-app__table"
          />

          <div v-if="!hidePagination">
            <base-pagination
              :pages="totalPages"
              :selected-page="page"
              @change="changePage"
              class="table-app__pagination"
            />
          </div>
        </template>

          <template v-else>
            <p>Empty list!</p>
          </template>
      </template>

      <template v-else>
        <p>Error Server!</p>
      </template>
    </template>
  </div>
</template>

<script>
import BaseInput from "~/components/UI/BaseInput.vue";
import BaseTable from "~/components/UI/BaseTable.vue";
import BasePagination from "~/components/UI/BasePagination.vue";

const LIMIT_TODOS = 25;
export default {
  name: 'IndexPage',
  components: {
    BasePagination,
    BaseTable,
    BaseInput,
  },
  data() {
    return {
      todos: [],
      searchQuery: '',
      page: 1,
      totalPages: 0,
    }
  },
  async fetch() {
    await this.fetchTodos();
  },
  fetchOnServer: false,
  computed: {
    filteredPosts() {
      return [...this.todos].filter((post) => post.title
        .toLowerCase()
        .includes(this.searchQuery.toLowerCase()));
    },
    todosLoaded() {
      return this.todos.length;
    },
    hidePagination() {
      return this.searchQuery.length;
    }
  },
  methods: {
    async fetchTodos() {
      try {
        const { data, headers } = await this.$axios.get('https://jsonplaceholder.typicode.com/todos', {
          params: {
            _page: this.page,
            _limit: LIMIT_TODOS,
          }
        });
        this.totalPages = Math.ceil(headers['x-total-count'] / LIMIT_TODOS);
        this.todos = [...data];
      } catch (err) {
        console.error(err);
      }
    },
    changePage(pageNumber = Number()) {
      this.page = pageNumber;
    }
  },
  watch: {
    page() {
      this.fetchTodos();
    },
  }
}
</script>

<style lang="scss">
$common-margin: 20px;

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
  padding: 20px;
}

.table-app {
  &__input {
    margin-bottom: $common-margin;
  }
  &__table {
    margin-bottom: $common-margin;
  }
}
</style>
