<template>
  <div id="app">
    <Timer ref="timer"></Timer>
    <button id='start_game' @click="startHandler(5)">Start</button>
    <router-link to="/result">GO TO RESULT</router-link>
    <p>{{ tokens }}</p>

    <input type="text" id="answerInput" v-on:keyup.enter="checkAnswer" v-model="answer">
    <p id="result">{{ result }}</p>

    <p>{{ userAnswers }}</p>
    <p>{{ correctAnswers }}</p>

    <TrainingResult v-bind:scores="scores"></TrainingResult>
    <router-view></router-view>
  </div>
</template>

<script>
import Timer from './components/Timer.vue';
import TrainingResult from './components/TrainingResult.vue';


export default {
  name: 'app',
  data: function() {
    return {
      tokens: [],
      userAnswers: [],
      correctAnswers: [],
      answer: '',
      result: '',
      scores: [],
    }
  },
  components: {
    Timer,
    TrainingResult
  },
  methods:{
    startHandler(e){
      this.$refs.timer.startCount(e)
      this.generateTokens();
      document.getElementById('start_game').disabled = true;
    },
    mathRandom: function(max, min){
      return Math.floor(Math.random() * (max + 1 - min) + min);
    },
    generateNumbersForAdd: function() {
      this.tokens.push(this.mathRandom(99,1));
      this.tokens.push(this.mathRandom(99,1));
    },
    generateNumbersForSubtraction: function() {
      this.tokens.push(this.mathRandom(99,1));
      this.tokens.push(this.mathRandom(99,1));
      this.tokens.sort(
        function(a, b) {
          return (a < b ? 1 : -1);
        });
    },
    generateNumbersForMultiplication: function() {
      this.tokens.push(this.mathRandom(99,1));
      this.tokens.push(this.mathRandom(10,1));
    },
    generateNumbersForDivision: function(){
      this.tokens.push(this.mathRandom(99,1));
      this.tokens.push(this.mathRandom(9,2));
      if(this.tokens[0] % this.tokens[1] !=0){
        this.tokens.length = 0
        this.generateNumbersForDivision()
      }
    },
    generateTokens: function(){
      let rand = this.mathRandom(3,0);
      let calc = ['+', '-', '*', '/']
      let operator = calc[rand];
      if(operator == "+") {
        this.generateNumbersForAdd()
      } else if(operator == "-"){
        this.generateNumbersForSubtraction()
      } else if(operator=="*"){
        this.generateNumbersForMultiplication()
      } else {
        this.generateNumbersForDivision()
      }
      this.tokens.unshift(operator)
    },
    checkAnswer: function(){
      let number = 0;
      switch(this.tokens[0]){
        case '+':
          number = this.tokens[1] + this.tokens[2]
          break;
        case '-':
          number = this.tokens[1] - this.tokens[2]
          break;
        case '*':
          number = this.tokens[1] * this.tokens[2]
          break;
        case '/':
          number = this.tokens[1] / this.tokens[2]
          break;
      }
      this.result = number == this.answer ? 'collect answer' : 'wrong answer';
      this.scores.push(number == this.answer)
      // use these array in result page.
      this.userAnswers.push(number);
      this.correctAnswers.push(this.answer);
      this.clearGen();
    },
    // Make a tokens array and answer variable clear
    clearGen: function(){
      this.tokens.length = 0;
      this.answer = ''
      this.generateTokens();
    }
  }
}
</script>

<style>

</style>
