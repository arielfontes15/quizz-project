<template>

  <div class="menu">
    <PlacarQuizz
      :countCorrect="this.countCorrect"
      :countIncorrect="this.countIncorrect"
    >
    </PlacarQuizz>
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <div>
        <template v-for="(answer, key) in this.answers" :key="key">
          <input 
            :disabled="this.answerSubmitted"
            type="radio"
            name="options"
            :value="answer"
            v-model="chosenAnswer"
          >
          <label :for="answer" v-html="answer"></label><br>
        </template>
      </div>
      <button
        v-if="!this.answerSubmitted"
        @click="submitAnswer()"
        class="btnEnviar"
        type="button"
      >Enviar</button>

      <section
        v-if="this.answerSubmitted"
        class="result"
      >
        <h4
          v-if="this.correctanswer == this.chosenAnswer"
          v-html="'⭕ Congratulations you got it right, click on the button for the next question.'"
        >
        </h4>
        <h4
          v-else
          v-html="`❌ I'm sorry, you picked the wrong answer. The correct is '${this.correctanswer.toUpperCase()}'`"
        >
        </h4>
        <button
          class="btnEnviar"
          type="button"
          @click="getDataSource()"
        >Next question</button>
      </section>
    </template>
  </div>
</template>

<script>
import PlacarQuizz from "@/components/PlacarQuizz.vue"

export default {
  name: 'App',
  components: {
    PlacarQuizz
  },
  data() {
    return {
      question: undefined,
      incorrectanswers: undefined,
      correctanswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      countIncorrect: 0,
      countCorrect: 0
    }
  },
  created() {
    // Acionar o hook created ira utilizar meu metodo
    // que puxas os dados da minha API
    this.getDataSource();
  },
  methods: {
    //Buscando os dados da minha apis
    //Utilizando o Axios
    getDataSource(){
      this.$http
      .get("https://opentdb.com/api.php?amount=10&category=18")
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectanswers = response.data.results[0].incorrect_answers;
        this.correctanswer = response.data.results[0].correct_answer;
      })
      //Redefinindo algumas propriedades antes de pegar meu dados da API
      this.answerSubmitted = false;
      this.chosenAnswer = null;
      this.question = null;
    },
    //Shuffle utilizado para randomizar as respostas
    shuffle(array) {
      array.sort(() => Math.random() - 0.5);
    },
    submitAnswer(){
      if(!this.chosenAnswer){
        window.alert('Pick one of the options');
      }else{
        this.answerSubmitted = true;
        if(this.chosenAnswer.includes(this.correctanswer)){
          this.countCorrect++;
          console.log('You got it!')
        }else{
          this.countIncorrect++;
          console.log('You got it wrong!')
        }
      }
    }
  },
  computed: {
    //Colocando as respostas corretas e incorretas em um array
    // e aplicando meu metodo Shuffle para randomizar as respostas
    answers() {
      //Transformando a minha resposta em OBJETO novamente
      //Evitando o Two-way-databinding
      //para quando alterarmos o ANSWER nao altere as demais propriedades
      let answer = JSON.parse(JSON.stringify(this.incorrectanswers));
      answer.push(this.correctanswer);
      this.shuffle(answer);
      return answer
    }
  }
}
</script>

<style lang="scss">
body {
    background: rgb(182,153, 153);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #0b0d0f;
  width: 100%;
  height: 100vh;

  .placar{
    margin: 20px;
    height: 80px;
    div span {
      position:absolute;
      top:50%;
      left:50%;
      transform:translate(-50%,-50%)
    }
    
    .itensPlacar{
        color: bold;
        font-size: 1.5rem;
        display: block;
    }
  }

  .menu {
    background: linear-gradient(#5896d3, rgb(182,153, 153));
    border-radius: 4px;
    padding-top: 20px;

    
    .btnEnviar{
      box-shadow: rgba(0, 0, 0, 0.5);
      border: none;
      width: 150px;
      height: 35px;
      border-radius: 4px;
      cursor: pointer;
      margin: 20px auto;
      background-color: rgb(212, 201, 218);
    }
  }
  
  input[type=radio] {
    display: inline-block;
    margin: 12px 4px;
  }
}
</style>
