<template>
<div>
<Score  :winAnswer="winAnswer"  :loseAnswer="loseAnswer"/>

<template v-if="this.questions">
  <h1>{{questions}}</h1>

  <template  :key="index" v-for="(answer , index) in this.answers">
    <input
        :disabled="this.answerSubmitted"
        type="radio"
        :value="answer"
        name="options"
        v-model="this.chosenAnswer"
    >
    <label >{{answer}}</label><br>
  </template>

  <button v-if="!this.answerSubmitted" @click="submitAnswer()" class="send" type="button">send</button>

  <section class="result" v-if="this.answerSubmitted">
    <h4 v-if="this.chosenAnswer === this.correctAnswer"
    v-html="'&#9989; Congratulation ,the answer '+this.correctAnswer+' is correct.'">
    </h4>

    <h4 v-else v-html="' &#10060; im sorry, you picked the wrong answer , the correct is :'+ this.correctAnswer+''" >
    </h4>
    <button @click="this.getNewQuestion()" class="send">Next question</button>

  </section>

</template>


</div>
</template>

<script>
import Score from "./components/Score";



export default {
  name: 'App',
  components: {Score},
  data(){
    return{
      name:'hello',
      questions:undefined,
      correctAnswer:undefined,
      incorrectAnswer:"",
      chosenAnswer:"",
      answerSubmitted: false,
      winAnswer:0,
      loseAnswer:0

    }
  },
  computed:{
    answers(){
      let answer = JSON.parse(JSON.stringify(this.incorrectAnswer))
      answer.splice(Math.round(Math.random() * answer.length), 0 ,this.correctAnswer);
      return answer
    }
  },
  created: function () {
    this. getNewQuestion()
  },
  methods:{
    submitAnswer(){
     if(!this.chosenAnswer){
      alert('full input')
     }if (this.chosenAnswer === this.correctAnswer){
       this.winAnswer++
       this.answerSubmitted = true

     }
  else{
        this.loseAnswer++
        this.answerSubmitted = true
      }
    },
    getNewQuestion(){
      this.answerSubmitted= false
      this.chosenAnswer = undefined
      this.questions = undefined
      this.axios.get("https://opentdb.com/api.php?amount=1&category=18").then((result) => {
        this.questions = result.data.results[0].question
        this.correctAnswer = result.data.results[0].correct_answer;
        this.incorrectAnswer = result.data.results[0].incorrect_answers;

      })
    }
  }

}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"]{
    margin: 12px 14px;
  }

  button.send{
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;

  }
}
</style>
