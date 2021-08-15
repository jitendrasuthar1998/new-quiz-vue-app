<template>
  <div class="question-box-container">
    <div>
      <b-jumbotron>
        <template #lead>
          {{ currentQuestion.question }}
        </template>

        <hr class="my-4" />

        <b-list-group>
          <b-list-group-item
            v-for="(answer, index) in answers"
            :key="index"
            @click.prevent="selectAnswer(index)"
            :class="answerClass(index)"
            >{{ answer }}</b-list-group-item
          >
        </b-list-group>

        <b-button
          variant="primary"
          @click="submitAnswer"
          :disabled="selectedIndex === null || answered"
          >Submit</b-button
        >
        <b-button @click="next" variant="success" href="#">Next</b-button>
      </b-jumbotron>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false,
    };
  },
  computed: {
    answers() {
      console.log("currentQuestion is", this.currentQuestion);
      let answers = this.currentQuestion.incorrect_answers;
      console.log("correct answer is", this.currentQuestion.correct_answer);
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log("selectedIndex of answer of is", this.selectedIndex);
      // console.log('index of this answer is', index)
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      }

      else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      }

      else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = "incorrect";
      }

      return answerClass;
    },
  },
  mounted() {
    this.shuffleAnswers();
  },
};
</script>

<style scoped>
.lead {
  margin: 10px;
  margin-top: 30px;
  font-size: 30px;
}
.list-group {
  margin-bottom: 20px;
  font-size: 20px;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.btn {
  margin: 10px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>
