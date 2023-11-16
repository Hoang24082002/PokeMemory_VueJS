<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 64) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <CardItem
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
      />
    </div>
  </div>
</template>

<script>
import CardItem from "./CardItem.vue";
export default {
  components: {
    CardItem,
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) {
        return false;
      }
      this.rules.push(card);
      if (this.rules.length === 2) {
        if (this.rules[0].value === this.rules[1].value) {
          console.log("TRUE");
          // add class disable in card + reset rule[]
          this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
          this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();
          this.rules = [];
          const disableElemnts = document.querySelectorAll(
            ".screen .card.disable"
          );
          if (
            disableElemnts &&
            disableElemnts.length === this.cardsContext.length - 2
          ) {
            setTimeout(() => {
              this.$emit("onFinish");
            }, 920);
          }
          console.log(disableElemnts);
        } else {
          console.log("FALSE");
          // close two card + reset rule[]
          setTimeout(() => {
            this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
            this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
            this.rules = [];
          }, 800);
        }
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped lang="css">
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background: var(--dark);
  color: var(--light);
}
.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
