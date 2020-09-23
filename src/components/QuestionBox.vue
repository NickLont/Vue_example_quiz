<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          :class="answerClass(index)"
          @click="selectAnswer(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        href="#"
        :disabled="(!selectedIndex || answered)"
        @click="submitAnswer"
      >
        Submit
      </b-button>
      <b-button
        variant="success"
        @click="getNextAnswer"
      >
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import { shuffle } from 'lodash'

export default {
  props: {
    currentQuestion: {
      type: Object,
      default () {
        return {}
      }
    },
    getNextAnswer: {
      type: Function,
      default: () => {}
    },
    increment: {
      type: Function,
      default: () => {}
    }
  },
  data () {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: null
    }
  },
  computed: {
    answers () {
      const answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      return answers
    }
  },
  // watcher for changes in props
  watch: {
    currentQuestion: {
    // runs as soon as the component gets mounted
      immediate: true,
      handler () {
        this.selectedIndex = null
        this.suffleAnswers()
        this.answered = false
      }
    }
  },
  methods: {
    selectAnswer (index) {
      this.selectedIndex = index
    },
    submitAnswer () {
      let isCorrect = false
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true
      }
      this.increment(isCorrect)
      this.answered = true
    },
    suffleAnswers () {
      const answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = shuffle(answers)
      this.correctIndex = this.shuffledAnswers.findIndex(answer => answer === this.currentQuestion.correct_answer)
    },
    answerClass (index) {
      let answerClass = ''
      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected'
      } else if (
        this.answered && this.correctIndex === index
      ) {
        answerClass = 'correct'
      } else if (
        this.answered && this.selectedIndex === index && this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
  }
}
</script>

<style scoped lang="scss">
    .list-group {
        margin-top: 15px;
        margin-bottom: 15px;
    }
    .list-group-item {
        margin-bottom: 5px;

        &:hover {
            background: #EEE;
            cursor: pointer;
        }
    }

    .selected {
        background: lightblue;
    }

    .correct {
        background: lightgreen;
    }

    .incorrect {
        background: red;
    }
    .btn {
        margin: 0 5px;
    }
</style>
