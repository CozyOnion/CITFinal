<template>
      <!-- navigation bar to be able to jump to a certain place on the page -->
      <header>
    <nav class="navbar">
      <img
        class="logoimg"
        src="https://cdn.glitch.global/5652955c-8d35-446f-ac37-dfeb59960913/3dgifmaker58386.gif?v=1650058474398"
        alt="logo"
      /><a href="#one">About</a>
      <a href="#two">How to Play</a>
      <a href="#three">Play Now</a>
      <a href="#four">Developers</a>
    </nav>
  </header>

  <main>
    <section class="picture">
      <div class="hangimg">
        <div class="content-wrap">
        </div>
      </div>
    </section>
    <section class="about">
      <div class="content-wrap">
        <h2 id="one">About</h2>
        <p>
          Hangman is a guessing game for two or more players. One player
          thinks of a word or sentence and the other tries to guess it by
          suggesting letters within a certain number of guesses. Usually this
          is played on paper and pencil, but this is an electronic version. In
          this online version, a word will be automatically generated for the
          player to guess.
        </p>
      </div>
    </section>
    <!--How to play the game -->
    <section class="htplay">
      <div class="content-wrap">
        <h2 id="two">How to Play</h2>
        <p>
          1. The host, in this case the computer, will generate a random word.
          Blank lines will appear to indicate how many letters are in the
          word.
        </p>
        <p>
          2. The player will start guessing letters that they think make up
          the word. Note that they do not have unlimited tries.
        </p>
        <p>
          3. Whenever the player guesses a correct letter, the computer will
          automatically put down in the blank spaces where that letter is in
          the word. If there are multiples of the guessed letter, the computer
          will also reveal it.
        </p>
        <p>
          4. If you guess a letter that is not in the word, the stick figure
          will slowly start to appear. For the first time wrong, the head will
          appear. The second time, the body will show up, Then the arms, legs,
          etc. When the man has fully appeared, that means you have lost.
        </p>
        <p>
          5. If you have guessed the word correctly, then you have won that
          round. A new word will appear after for you yo try again.
        </p>
      </div>
    </section>
    </main>
  <Header />
  <div class="game-container">
    <Figure :wrong-count="wrongLetters.length" />
    <WrongLetters :wrong-letters="wrongLetters" />
    <Word :letters="letters" :correct-letters="correctLetters" />
  </div>
  <Popup :status="status" :word="word" @reset="reset" />
  <!--<Notification :show="notification" />-->
  <section class="Our-Developers">
    <div class="developimg">
      <div class="content-wrap">
        <h2 id="four">Our Developers</h2>
        <div class="column">
          <img
            class="berj"
            src="https://cdn.glitch.global/5652955c-8d35-446f-ac37-dfeb59960913/Screenshot%202022-05-16%20183537.jpeg?v=1652751727506"
            alt="first"
            style="width: 200px"
          />
          <p>Berj Kozanian</p>
        </div>
        <div class="column">
          <img
            class="fern"
            src="https://cdn.glitch.global/5652955c-8d35-446f-ac37-dfeb59960913/8DF68AF8-080A-44F8-ABCA-502B2615F7DD.jpeg?v=1652752757214"
            alt="second"
            style="width: 200px"
          />
          <p>Fernando Reyes</p>
        </div>
        <div class="column">
          <img
            src="https://cdn.hyperdev.com/paste-me.svg?v=1477325869954"
            alt="third"
            style="width: 200px"
          />
          <p>first name last name</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { computed, ref } from 'vue'

import './assets/style.css'

import Header from './components/HeaderMain'
import Figure from './components/FigureMain'
import WrongLetters from './components/WrongLettersMain'
import Word from './components/WordMain'
import Popup from './components/PopupMain'  
//import Notification from './components/NotificationMain'
import onKeydown from './assets/onKeydown'
const words = ["programming"]
let iter = 0;
const randomWord = () => {
    fetch(`https://random-words-api.vercel.app/word`)
    .then(response => response.json())
    .then((data) => {
      words.push(data[0].word.toLowerCase())
    })
    .catch((err) => {
        console.log("error:", err)
    })
    console.log(iter)
    console.log(words)
    return words[iter]
}

export default {
  components: { Header, Figure, Word, WrongLetters, Popup /*, Notification*/ },
  setup() {
    let word = ref(randomWord())
    const guessedLetters = ref([])
    
    const letters = computed(() => word.value.split(''))
    const wrongLetters = computed(() =>
      guessedLetters.value.filter(l => !letters.value.includes(l))
    )
    const correctLetters = computed(() =>
      guessedLetters.value.filter(l => letters.value.includes(l))
    )

    const status = computed(() => {
      if (wrongLetters.value.length === 6) return 'lose'
      if (letters.value.every(l => correctLetters.value.includes(l)))
        return 'win'
      return ''
    })
    const reset = () => {
      iter++
      guessedLetters.value = []
      word.value = randomWord()
    }

    const notification = ref(false)
    const showNotification = () => {
      notification.value = true
      setTimeout(() => (notification.value = false), 2000)
    }

    onKeydown(event => {
      const letter = event.key.toLowerCase()
      if (event.keyCode < 65 || event.keyCode > 90) return
      if (status.value) return
      if (guessedLetters.value.includes(letter)) {
        showNotification()
        return
      } 
      guessedLetters.value.push(letter)
    })

    return {
      letters,
      word,
      wrongLetters,
      correctLetters,
      guessedLetters,
      notification,
      status,
      reset
    }
  }
}
</script>