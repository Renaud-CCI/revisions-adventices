<template>
  <section class="container">

    <div class="score-div">
      <OrnamentalScoreContainer :OrnamentalIsRevision="OrnamentalIsRevision" />
    </div>

    <div class="pt-6">
      <OrnamentalToggleSwitch @ornamental-toggle-mode="handleToggleMode" />
    </div>

    <div class="refresh-div flex justify-center items-center">
      <button class="text-white bg-sky-600 rounded" @click="refreshPage">Réinitialiser</button>
    </div>

      <OrnamentalCard v-if="OrnamentalIsRevision" v-for="(ornamental, index) in ornamentalsArray" :key="key" :ornamental="ornamental" />

      <OrnamentalQuizzCardContainer v-if="!OrnamentalIsRevision" v-for="(ornamental, index) in ornamentalsArray" :key="index" :index="index" :ornamentalsArray="ornamentalsArray" />
    
    <div class="refresh-div flex justify-center items-center">
      <button 
        class="text-white px-4 py-1 rounded" 
        :class="OrnamentalIsRevision ? 'bg-sky-600' : 'bg-amber-500'" 
        @click="refreshClick"
      >
        Réinitialiser
      </button>
    </div>

  </section>

  

</template>

<script>
import smoothscroll from 'smoothscroll-polyfill';
smoothscroll.polyfill();
import OrnamentalScoreContainer from './OrnamentalsComponents/OrnamentalScoreContainer.vue';
import OrnamentalToggleSwitch from './OrnamentalsComponents/OrnamentalToggleSwitch.vue';
import OrnamentalCard from './OrnamentalsComponents/OrnamentalCard.vue';
import OrnamentalQuizzCardContainer from './OrnamentalsComponents/OrnamentalQuizzCard/OrnamentalQuizzCardContainer.vue';
import ornamentals from '../assets/ornamentals.json';

export default {
  name: 'OrnamentalsLearningContainer',
  components: {
    OrnamentalScoreContainer,
    OrnamentalToggleSwitch,
    OrnamentalCard,
    OrnamentalQuizzCardContainer
  },
   data() {
    return {
      ornamentalsArray: this.shuffle(Object.values(ornamentals)),
      OrnamentalIsRevision: true,
    };
  },
  methods: {
    shuffle(array) {
      let currentIndex = array.length, temporaryValue, randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {

        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    },
    refreshPage() {
      window.location.reload();
    },
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: 'smooth' });
    },
    refreshClick() {
      this.scrollToTop();
      setTimeout(() => {
        this.refreshPage();
      }, 800);
    },
    handleToggleMode() {
      this.OrnamentalIsRevision = !this.OrnamentalIsRevision;
      const button = document.querySelector('.refresh-div button');
      const isMobile = window.matchMedia('(max-width: 768px)').matches;
      if (this.OrnamentalIsRevision) {
        button.style.opacity = "1";
        button.style.marginTop = "1rem";
        button.style.height = isMobile ? "2rem" : "3rem";
        button.style.pointerEvents = "auto";
      } else {
        button.style.opacity = "0";
        button.style.marginTop = "0";
        button.style.height = "0";
        button.style.pointerEvents = "none";
      }
    }
  }
}

</script>

<style scoped>
.container {
  padding-top: 3rem;
}

.refresh-div button {
  font-size: 1.2rem;
  margin-top: 1rem;
  height: 3rem;
  padding: 0.5rem 1rem;
  transition: opacity 0.5s, margin-top 0.5s, height 0.5s;
}

@media screen and (max-width: 768px) {
  .refresh-div button {
    font-size: 0.8rem;
    height: 2rem;
    padding: 0.2rem 1rem;
  }
} 
</style>
