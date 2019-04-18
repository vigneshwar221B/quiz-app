<template>
  <div id="app">
  <Header :score = "score"></Header>

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm=6 offset=3>
          <Question v-if="answers.length > 0 && canswers.length > 0 && questions.length > 0 " 
            :question="questions[index]" 
            :next = "next" 
            :answers="answers[index]" 
            :canswer="canswers[index]" 
            :quesnum = "quesnum" 
            :updateScore ="updateScore">
            </Question>
        </b-col>
      </b-row>
    </b-container>
  </div>
  
</template>

<script>
  import Header from './components/Header.vue'
  import Question from './components/Question.vue'
  import {
    log
  } from 'util';

  export default {
    name: 'app',
    components: {
      Header,
      Question
    },
    data() {
      return {
        questions: [],
        answers : [],
        canswers: [],
        index : 0,
        score : 0,
        quesnum : 1,
        
      }
    },
    methods : {
      next(){
        this.index += 1;
        this.quesnum += 1
      },
      updateScore(num){
        this.score = num
      }
    },
    mounted: function () {
   
      fetch("https://opentdb.com/api.php?amount=10&category=31&difficulty=easy&type=multiple", {
          method: 'get'
        })
        .then((res) => {
          return res.json();
        })
        .then((jsonData) => {
          
        for (var i =0; i< jsonData.results.length; i++){
          
           this.questions.push(jsonData.results[i].question);
           var temp = jsonData.results[i].incorrect_answers;
           temp.push(jsonData.results[i].correct_answer);
           this.answers.push(temp);
           this.canswers.push(jsonData.results[i].correct_answer);

        }
         
        })
        .catch((err) => {
          console.log(err);
        });
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>