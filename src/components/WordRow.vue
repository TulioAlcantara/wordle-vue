<template>
  <div class="grid max-w-xs grid-cols-5 gap-1 mx-auto mb-1">
    <LetterBox
      v-for="i in 5"
      :key="i"
      :letter="value ? value[i - 1] : ''"
      :color="colors[i - 1]"
    >
    </LetterBox>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
import LetterBox from "./LetterBox.vue";

watch(
  () => props.submitted,
  async (submitted, prevSubmitted) => {
    if (submitted) {
      let solution = props.solution;
      let value = props.value;

      let temp = ["gray", "gray", "gray", "gray", "gray"];
      let letterPool = [];
      for (let i = 0; i < 5; i++) {
        if (solution?.charAt(i) == value?.charAt(i)) {
          temp[i] = "green";
        } else {
          letterPool.push(solution?.charAt(i));
        }
      }

      for (let i = 0; i < 5; i++) {
        if (temp[i] == "gray") {
          if (letterPool.indexOf(value?.charAt(i)) != -1) {
            temp[i] = "yellow";
            letterPool.splice(letterPool.indexOf(value?.charAt(i)), 1);
          }
        }

        colors.value[i] = temp[i];
        await new Promise((resolve) => setTimeout(resolve, 100));
      }
    }
  }
);

const props = defineProps({
  value: String,
  solution: String,
  submitted: Boolean,
});

const colors = ref(["", "", "", "", ""]);
</script>

<style lang="css" scoped></style>
