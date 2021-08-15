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
            :class="[selectedIndex === index ? 'selected' : null]"
            >{{ answer }}</b-list-group-item
          >
        </b-list-group>

        <b-button variant="primary" href="#">Submit</b-button>
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
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
    };
  },
  computed: {
    answers() {
      console.log("currentQuestion is", this.currentQuestion);
      let answers = this.currentQuestion.incorrect_answers;
      console.log("currect answer is", this.currentQuestion.correct_answer);
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
