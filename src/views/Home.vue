<template>
  <div class="container">
    <h1 class="app-title">
      Github <span class="app-title_regular">Jobs</span>
    </h1>
    <SearchBar @search="search" />
    <main class="main-section">
      <div class="controls">
        <label for="full-time" class="controls__label">
          <input type="checkbox" id="full-time" v-model="fullTime" />
          Full time</label
        >
        <span class="header">Location</span>
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

      <div class="vacancies">
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
          @onClick="() => onCardClick(vacancy)"
        />
      </div>
    </main>
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

    onCardClick(vacancy) {
      this.$router.push(`/vacancy/${vacancy.id}`);
    },

    getFormattedDate(dateString) {
      const date = new Date(dateString);
      const now = new Date();
      let range = now - date;
      let years = 0;
      let months = 0;
      let days = 0;
      const yearInMs = 31556952000;
      const monthInMs = 2592000000;
      const dayInMs = 86400000;

      years = range > yearInMs ? Math.trunc(range / yearInMs) : 0;
      range -= Math.round(years * yearInMs);
      months = range > monthInMs ? Math.trunc(range / monthInMs) : 0;
      range -= Math.round(months * monthInMs);
      days = range > dayInMs ? Math.trunc(range / dayInMs) : 1;

      let result = `${days} days ago`;
      result = months ? `${months} months, ${result}` : result;
      result = years ? `${years} years, ${result}` : result;
      return result;
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

.main-section {
  display: flex;
  flex-flow: column nowrap;
}

.controls {
  display: flex;
  flex-flow: column nowrap;
  padding: 1rem 0;
}

.header {
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

.vacancies {
  display: flex;
  flex-flow: column nowrap;
}

.vacancy-card {
  margin-bottom: 1.25rem;
}

@media (min-width: 768px) {
  .container {
    padding: 2vh 10vw;
  }

  .app-title {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  .main-section {
    flex-flow: row nowrap;
  }

  .controls {
    width: 30%;
  }

  .vacancies {
    width: 70%;
    padding: 1rem 0 2rem 2rem;
  }
}
</style>
