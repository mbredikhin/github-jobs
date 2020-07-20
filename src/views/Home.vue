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

    <VacancyPreview
      class="vacancy-card"
      v-for="vacancy in vacancies"
      :key="vacancy.id"
      :company="vacancy.company"
      :company_logo="
        vacancy.company_logo ||
          'https://via.placeholder.com/250x250?text=Image+not+found'
      "
      :created_at="getFormattedDate(vacancy.created_at)"
      :location="vacancy.location"
      :title="vacancy.title"
      :id="vacancy.id"
      @onClick="onCardClick"
    />
  </div>
</template>

<script>
import SearchBar from '@/components/SearchBar.vue';
import VacancyPreview from '@/components/VacancyPreview.vue';

export default {
  name: 'Home',

  components: {
    SearchBar,
    VacancyPreview,
  },

  data() {
    return {
      vacancies: [],
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
      Object.entries(this.cities).forEach(([key, value]) => {
        if (value) {
          location += `+${key}`;
        }
      });
      try {
        const url = `https://cors-anywhere.herokuapp.com/https://jobs.github.com/positions.json?search=${query}&location=${location}&full_time=${this.fullTime}`;
        const response = await fetch(url);
        this.vacancies = await response.json();
      } catch (error) {
        console.log('Something went wrong 😔 ', error);
      }
    },

    onCardClick(id) {
      console.log(id);
    },

    getFormattedDate(dateString) {
      const date = new Date(dateString);
      const days = Math.trunc((Date.now() - date) / 86400000);
      return `${days} days ago`;
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

.vacancy-card {
  margin-bottom: 1.25rem;
}
</style>
