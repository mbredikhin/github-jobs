<template>
  <div class="search">
    <div class="search-bar">
      <input
        type="search"
        placeholder="Title, companies, expertise or benefits"
        class="search-bar__input"
        v-model="query"
        @keydown.enter="$emit('search', query)"
      />
      <input
        type="submit"
        value="Search"
        class="search-bar__submit"
        @click="$emit('search', query)"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: '',
    };
  },
};
</script>

<style>
.search {
  background: url('../assets/skyscrapers.png') center / cover no-repeat;
  padding: 2.5rem 1rem;
  border-radius: 6px;
}

.search-bar {
  display: flex;
  justify-content: space-between;
  padding: 0.25rem;
  background: white;
  border-radius: 4px;
}

.search-bar__input {
  font-family: 'Roboto', sans-serif;
  width: 100%;
  padding: 0.5rem;
  border: none;
  border-radius: 4px;
  margin-right: 0.25rem;
  font-size: 0.75rem;
  text-overflow: ellipsis;
}

.search-bar__submit {
  padding: 0.75rem 1.5rem;
  background: #4e86ff;
  border: none;
  border-radius: 4px;
  color: white;
  font-family: 'Roboto', sans-serif;
  font-size: 0.85rem;
  font-weight: 500;
}

@media (min-width: 768px) {
  .search {
    padding: 2rem 10vw;
  }
}
</style>
