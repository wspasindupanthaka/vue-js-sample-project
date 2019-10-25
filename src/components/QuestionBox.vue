<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="answer in pushAnswers"
          :key="answer"
          @click="selectAnswer(answer)"
          :class="[!answered && 
          !answered && selectedAnswer===answer ? 'selected' : 
          answered && currentQuestion.correct_answer===answer ? 'correct' : 
          answered && selectedAnswer===answer && currentQuestion.correct_answer!==answer ? 'incorrect' :'']"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedAnswer==='' || answered"
      >Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedAnswer: "",
       correctIndex:90,
      answered:false
    }; 
  },
  watch : {
      currentQuestion : {
          handler() {
              this.selectedAnswer="",
              this.answered=false
          }
      }
  },
  methods: {
    selectAnswer(answer) {
      console.log(answer);
      this.selectedAnswer = answer;
    },
    submitAnswer() {  
        
        let correct=false

        if(this.selectedAnswer===this.currentQuestion.correct_answer)
            correct=true;
        
        this.answered=true
        this.increment(correct) 
    }
  },
  computed: {
    pushAnswers() {
      let answers = this.currentQuestion.incorrect_answers;
      answers.push(this.currentQuestion.correct_answer);   
     
      return answers; 
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: lightcoral;
}
</style>