<template>
  <div class="word">
    <!--For each letter in the letters array display a span
    also check if that letter is in the correct letters array
    if it is, then display the letter, if not return an empty string
    -->
    <span class="letter" v-for="(letter, i) in letters" :key="i">
      {{ correctLetters.includes(letter) ? letter : '' }}
    </span>
  </div>
</template>

<script>
export default {
  //Here we define the array of letters and correct letters to be displayed
  props: {
    letters: { type: Array, required: true },
    correctLetters: { type: Array, default: () => [] }
  }
}
</script>
