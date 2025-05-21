<template>
  <div>
    <div class="numbers-limit">
      <label>Numbers Limit (Between: 1 - 1000):</label>
      <input type="number" v-model="limit" min="1" :max="1000" step="1" />
    </div>
    <div>
      <div
        v-for="number in generatedNumbers"
        :key="number"
        :id="'number-' + number"
        :class="{
          number: true,
          divisor: hoveredNumber && hoveredNumber % number === 0,
          hovered: hoveredNumber === number,
        }"
        @mouseover="hasHoveredNumber(number)"
        @mouseout="hasHoveredNumber(null)"
      >
        {{ number }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";

const limit = ref<number>(100); //limit of amount of generated numbers
const generatedNumbers = ref<number[]>([]); //list of generated numbers
const hoveredNumber = ref<number | null>(null); //hold currently hovered number

//ovbserve changes of the limit value
watch(
  limit,
  (newVal: number) => {
    //constraint to 1000 max for visual reason, too many numbers could be cumbersome to view
    if (newVal >= 1 && newVal <= 1000) {
      //implement some sort of debounce for ux
      setTimeout(() => {
        const numbers = Array.from({ length: newVal }, (_, index) => index + 1); //generate based on limit value
        generatedNumbers.value = numbers.sort(() => Math.random() - 0.5); //sort randomly
      }, 500);
    }
  },
  { immediate: true }
);

function hasHoveredNumber(number: number | null) {
  hoveredNumber.value = number; //Set the hovered numver value
}
</script>

<style>
.numbers-limit {
  margin-bottom: 10px;
}
input {
  margin-left: 8px;
  padding: 4px;
}

.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
}

.number.divisor {
  background-color: red; /* Hovered number highlighted in red*/
}
.number.hovered {
  background-color: darkgray; /* Hovered number highlighted in gray*/
}
</style>
