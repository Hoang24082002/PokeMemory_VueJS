<template>
  <main-screen
    v-if="statusMath === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMath === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMath === 'result'"
    :timer="timer"
    @onStartAgain="statusMath = 'default'"
  />
  <copy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";
export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMath: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(confix) {
      console.log(confix);
      this.settings.totalOfBlocks = confix.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      const saoTron = (list) => list.sort(() => Math.random() - 0.5);
      this.settings.cardsContext = saoTron(saoTron(saoTron(saoTron(cards))));
      console.log(this.settings.cardsContext);
      this.settings.startedAt = new Date().getTime();
      this.statusMath = "match";
    },
    onGetResult() {
      // get timme
      this.timer = new Date().getTime() - this.settings.startedAt;
      // switch Result components
      this.statusMath = "result";
    },
  },
};
</script>
