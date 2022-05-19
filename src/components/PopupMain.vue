<template>
  <div v-if="finalMessage" class="popup-container" id="popup-container">
    <div class="popup">
      <h2>{{ finalMessage }}</h2>
      <h3 v-show="status === 'lose'">...the word was: {{ word }}</h3>
      <button @click="reset">Play Again</button>
    </div>
  </div>
</template>

<script>
// props for status and word are initialized to an empty string

import { computed } from 'vue'
export default {
  props: {
    status: { type: String, default: '' },
    word: { type: String, default: '' }
  },
  setup(props, { emit }) {
    //final message is caclculated from props.status and will return either the losing 
    //or winning messsage
    //This is checked multiple times thats also why it has an empty return if neither if 
    //statement is made
    const finalMessage = computed(() => {
      if (props.status === 'win') return 'Congratulations! You won! 😃'
      if (props.status === 'lose') return 'Unfortunately you lost. 😕'
      return ''
    })
    const reset = () => emit('reset')
    return { finalMessage, reset }
  }
}
</script>
