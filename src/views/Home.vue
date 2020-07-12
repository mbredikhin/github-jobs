<template>
  <div>
    <h1 class="app-title">
      Github <span class="app-title_regular">Jobs</span>
    </h1>
    <SearchBar @search="search" />
    <div class="controls">
      <label for="full-time" class="controls__label">
        <input type="checkbox" id="full-time" v-model="fullTime" />
        Full time</label
      >
      <span class="controls__header">Location</span>
      <input
        type="text"
        class="controls__input"
        placeholder="City, state, zip code or country"
        v-model="location"
      />
      <label for="london" class="controls__label">
        <input type="checkbox" id="london" v-model="cities.london" />
        London</label
      >
      <label for="amsterdam" class="controls__label">
        <input type="checkbox" id="amsterdam" v-model="cities.amsterdam" />
        Amsterdam</label
      >
      <label for="new-york" class="controls__label">
        <input type="checkbox" id="new-york" v-model="cities.newYork" /> New
        York</label
      >
      <label for="berlin" class="controls__label">
        <input type="checkbox" id="berlin" v-model="cities.berlin" />
        Berlin</label
      >
    </div>
  </div>
</template>

<script>
import SearchBar from '@/components/SearchBar.vue';

export default {
  name: 'Home',

  components: {
    SearchBar,
  },

  data() {
    return {
      location: '',
      fullTime: false,
      cities: {
        london: false,
        amsterdam: false,
        newYork: false,
        berlin: false,
      },
    };
  },

  methods: {
    async search(query) {
      let { location } = this;
      Object.entries(this.cities).forEach((element) => {
        if (element.value) {
          location += `+${element.key}`;
        }
      });

      try {
        const url = `https://cors-anywhere.herokuapp.com/https://jobs.github.com/positions.json?search=${query}&location=${location}&full_time=${this.fullTime}`;
        const response = await fetch(url);
        const json = await response.json();
        console.log(json);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
.app-title {
  font-family: 'Poppins', sans-serif;
}

.app-title_regular {
  font-weight: 400;
}

.controls {
  display: flex;
  flex-flow: column nowrap;
  padding: 1rem 0;
}

.controls__header {
  font-family: 'Poppins', sans-serif;
  font-size: 0.9rem;
  font-weight: 600;
  text-transform: uppercase;
  color: #b9bdcf;
  line-height: 1.5;
  margin-top: 0.5rem;
}

.controls__label {
  display: inline-block;
  line-height: 2;
  font-family: 'Poppins', sans-serif;
  font-weight: 500;
  color: #334680;
}

.controls__input {
  font-family: 'Roboto', sans-serif;
  padding: 0.75rem;
  border: none;
  border-radius: 2px;
  margin-right: 0.25rem;
  font-size: 0.75rem;
  box-shadow: 1px 1px 3px #dcdfec;
  margin: 0.5rem 0;
}
</style>
