<template>
  <div class="keyboard"></div>
</template>

<script setup lang="ts">
import Keyboard from "simple-keyboard";
import "simple-keyboard/build/css/index.css";
import { ref, onMounted, watch } from "vue";

const props = defineProps({
  guessedLetters: {
    type: Object,
    default: () => ({
      miss: [],
      found: [],
      hint: [],
    }),
  },
});

const emit = defineEmits(["onKeyPress"]);

const keyboard = ref<Keyboard>();

const onKeyPress = (button: string) => {
  emit("onKeyPress", button);
};

onMounted(() => {
  keyboard.value = new Keyboard("keyboard", {
    layout: {
      default: [
        "q w e r t y u i o p",
        "a s d f g h j k l ",
        "{enter} z x c v b n m {bksp}",
      ],
    },
    onKeyPress: onKeyPress,
  });
});

watch(
  () => props.guessedLetters,
  (guessedLetters, prevGuessedLetters) => {
    keyboard.value?.addButtonTheme(guessedLetters.miss.join(" "), "miss");
    keyboard.value?.addButtonTheme(guessedLetters.hint.join(" "), "hint");
    keyboard.value?.addButtonTheme(guessedLetters.found.join(" "), "found");
  },
  { deep: true }
);
</script>

<sytle lang="css">
div.miss {
  @apply bg-gray-500 !important;
  @apply text-white !important;
}

div.found {
  @apply bg-green-500 !important;
  @apply text-white !important;
}

div.hint:not(.found) {
  @apply bg-yellow-500 !important;
  @apply text-white !important;
}
</sytle>
