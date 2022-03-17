<script setup lang="ts">
import Keyboard from "./components/Keyboard.vue";
import Header from "./components/Header.vue";
import { onMounted, reactive } from "vue";
import WordRow from "./components/WordRow.vue";

onMounted(() => {
  window.addEventListener("keydown", (event) => {
    event.preventDefault();
    let key = "";
    if (event.key === "Backspace") {
      key = "{bksp}";
    } else if (event.key === "Enter") {
      key = "{enter}";
    } else {
      key = event.key.toLowerCase();
    }
    handleInput(key);
  });
});

const state = reactive({
  solution: "books",
  guesses: ["", "", "", "", "", ""],
  currentGuessIndex: 0,
  guessedLetters: {
    miss: [] as string[],
    found: [] as string[],
    hint: [] as string[],
  },
});

const handleInput = (key: string) => {
  if (state.currentGuessIndex >= 6) {
    return;
  }
  const currentGuess = state.guesses[state.currentGuessIndex];

  if (key === "{enter}") {
    if (currentGuess.length === 5) {
      checkGuess(currentGuess);
      state.currentGuessIndex++;
    }
    return;
  }

  if (key == "{bksp}") {
    state.guesses[state.currentGuessIndex] = currentGuess.slice(
      0,
      currentGuess.length - 1
    );
    return;
  }

  if (currentGuess.length < 5) {
    const alphaRegex = /[a-z]/;
    if (alphaRegex.test(key)) {
      state.guesses[state.currentGuessIndex] += key;
    }
  }
};

const checkGuess = (guess: string) => {
  for (let i = 0; i < 5; i++) {
    const letter = guess[i];
    if (state.solution.includes(letter)) {
      if (state.solution[i] === letter) {
        !state.guessedLetters.found.includes(letter) &&
          state.guessedLetters.found.push(letter);
      } else {
        !state.guessedLetters.hint.includes(letter) &&
          state.guessedLetters.hint.push(letter);
      }
    } else {
      !state.guessedLetters.miss.includes(letter) &&
        state.guessedLetters.miss.push(letter);
    }
  }
};
</script>

<template>
  <Header></Header>
  <main class="flex flex-col h-screen max-w-md mx-auto justify-evenly">
    <div>
      <WordRow
        v-for="(guess, i) in state.guesses"
        :key="i"
        :value="guess"
        :solution="state.solution"
        :submitted="i < state.currentGuessIndex"
      ></WordRow>
    </div>
    <Keyboard
      @onKeyPress="handleInput"
      :guessedLetters="state.guessedLetters"
    ></Keyboard>
  </main>
</template>

<style></style>
