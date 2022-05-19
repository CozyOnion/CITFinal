<template>
  <div class="wrong-letters-container">
    <div class="wrong-letters">
      <!-- 
      This container will be displayed to the user and contain the wrong letters
      that the user has attempted

      v-show will show if the js eval returns true

      v-for will loop through all added wrong letters 
      display the wrong letter and conditionallyt add commas
      -->
      <p v-show="wrongLetters.length > 0">Wrong</p>
      <span v-for="(letter, i) in wrongLetters" :key="letter">
        {{ letter }}
        {{ i === wrongLetters.length - 1 ? '' : ', ' }}
      </span>
    </div>
  </div>
</template>

<script>
//here we define an array for the wrong letters and initialize it to empty
export default {
  props: {
    wrongLetters: { type: Array, default: () => [] }
  }
}
</script>
