<template>
  <div class="card" @click="$emit('onClick', id)">
    <div class="image">
      <img class="card__image" :src="company_logo" :alt="company" />
    </div>
    <div class="content">
      <span class="card__company">{{ company }}</span>
      <span class="card__title">{{ title }}</span>
      <div class="footer">
        <span v-if="isFullTime" class="card__badge">Full time</span>
        <div class="extras">
          <div class="card__extra card__extra--location">
            <img class="card__icon" :src="require('@/assets/world.svg')" />
            <span class="card__extra-text">{{ location }}</span>
          </div>
          <div class="card__extra card__extra--created">
            <img class="card__icon" :src="require('@/assets/clock.svg')" />
            <span class="card__extra-text">{{ created_at }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    company: {
      type: String,
      required: true,
    },

    company_logo: {
      type: String,
      required: true,
    },

    created_at: {
      type: String,
      required: true,
    },

    isFullTime: {
      type: Boolean,
      default: true,
    },

    location: {
      type: String,
      required: true,
    },

    title: {
      type: String,
      required: true,
    },

    id: {
      type: String,
      required: true,
    },
  },

  computed: {
    created() {
      return new Date() - this.created_at;
    },
  },
};
</script>

<style scoped>
.card {
  display: flex;
  padding: 0.5rem;
  background: white;
  border-radius: 4px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.05);
}

.card:hover {
  cursor: pointer;
}

.image {
  width: 85px;
  padding: 0.25rem;
}

.card__image {
  display: inline-block;
  width: 100%;
}

.content {
  padding: 0.25rem 0 0 0.5rem;
  width: 75%;
  display: flex;
  flex-flow: column nowrap;
  justify-content: space-between;
  font-family: 'Roboto', sans-serif;
}

.card__company {
  font-weight: bold;
  font-size: 12px;
  line-height: 14px;
  color: #334680;
}

.card__title {
  font-size: 16px;
  line-height: 19px;
  color: #334680;
  padding: 0.2rem 0;
}

.card__badge {
  display: inline-block;
  text-align: center;
  max-width: 4rem;
  padding: 0.25rem;
  font-size: 12px;
  color: #334680;
  font-weight: 500;
  border: 1px solid #334680;
  border-radius: 4px;
  margin: 0.5rem 0;
}

.card__location {
  font-size: 12px;
  color: #b9bdcf;
  border-radius: 4px;
}

.footer {
  display: flex;
  flex-flow: column nowrap;
  justify-content: space-between;
}

.card__extra {
  display: inline-flex;
  margin-right: 0.5rem;
  font-size: 12px;
  color: #b7bcce;
}

.card__extra-text {
  line-height: 2;
}

.card__icon {
  width: 18px;
  margin-right: 0.25rem;
}

@media (min-width: 768px) {
  .card__extra {
    float: right;
  }

  .footer {
    flex-flow: row nowrap;
  }

  .extras {
    padding: 0.5rem 0 0;
  }
}
</style>
