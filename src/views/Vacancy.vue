<template>
  <div class="container">
    <h1 class="app-title">
      Github <span class="app-title_regular">Jobs</span>
    </h1>
    <main class="vacancy-main"></main>
    <div class="column column--small">
      <button class="button" @click="$router.back()">
        🠔 Back to search
      </button>
      <span class="header">How to apply</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
};
</script>

<style>
.button {
  color: #1e86ff;
  font-family: Poppins;
  font-weight: 500;
  font-size: 14px;
  border: none;
  background: none;
}

.vacancy-main {
  display: flex;
  flex-flow: row wrap;
}

.column {
  display: flex;
  flex-flow: column nowrap;
}

/* .column--small {
  max-width: 375px;
} */
</style>
