<template lang="html">
  <div class="space-y-6">
    <div
      class="flex flex-col items-center justify-center max-w-sm rounded-xl shadow-lg py-6 px-4 space-y-4 bg-white"
    >
      <h2 class="text-2xl font-bold">{{ heroName }}</h2>
      <img src="" />
      <div class="w-full flex flex-col">
        <div
          class="flex justify-between uppercase text-xl font-bold text-red-600"
        >
          <h3>Physical strength</h3>
          <span>{{ heroStrength }}</span>
        </div>
        <div
          class="flex justify-between uppercase text-xl font-bold text-black"
        >
          <h3>Combat</h3>
          <span>{{ heroCombat }}</span>
        </div>
        <div
          class="flex justify-between uppercase text-xl font-bold text-blue-600"
        >
          <h3>Intelligence</h3>
          <span>{{ heroIntelligence }}</span>
        </div>
      </div>
      <p class="text-left">
        {{ heroOccupation }}
      </p>
    </div>
    <button
      class="py-4 px-8 bg-indigo-700 text-white rounded-md shadow-md"
      @click="fetchRandomHero"
    >
      next hero
    </button>
  </div>
</template>

<script>
import axios from 'axios';

const MAXIMUM_NUMBER_OF_HERO_ID = 750;
const MINIMUM_NUMBER_OF_HERO_ID = 1;
const SUPER_HERO_BASE_URL = 'https://superheroapi.com/api.php';
const ACCESS_TOKEN = '10159434919196340';
const UNKNOWN = '-';

export default {
  name: 'Hero',

  data() {
    return {
      hero: null || '',
    };
  },

  created() {
    console.log(this.fetchRandomHero());
  },

  computed: {
    heroName() {
      return this.hero?.name ?? UNKNOWN;
    },

    heroOccupation() {
      return this.hero.work.occupation === 'null'
        ? UNKNOWN
        : this.hero.work.occupation;
    },

    heroStrength() {
      return this.heroPowerStats.strength === 'null'
        ? UNKNOWN
        : this.heroPowerStats.strength;
    },

    heroPowerStats() {
      const { powerstats } = this.hero;
      return powerstats;
    },

    heroCombat() {
      return this.heroPowerStats.combat === 'null'
        ? UNKNOWN
        : this.heroPowerStats.combat;
    },

    heroIntelligence() {
      return this.heroPowerStats.intelligence === 'null'
        ? UNKNOWN
        : this.heroPowerStats.intelligence;
    },
  },

  methods: {
    randomHeroId() {
      return (
        Math.floor(
          Math.random() *
            (MAXIMUM_NUMBER_OF_HERO_ID - MINIMUM_NUMBER_OF_HERO_ID + 1)
        ) + MINIMUM_NUMBER_OF_HERO_ID
      );
    },

    async fetchRandomHero() {
      const id = this.randomHeroId();
      try {
        let response = await axios.get(
          `${SUPER_HERO_BASE_URL}/${ACCESS_TOKEN}/${id}`
        );
        this.hero = response.data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
