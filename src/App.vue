<template>
  <div class="menu">
    <h1 v-html="this.question"></h1>
    <div>
      <input type="radio" name="options" value="True">
      <label>True</label><br>
      <input type="radio" name="options" value="False">
      <label>False</label><br>
    </div>
    <button class="btnEnviar">Enviar</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      question: undefined,
      incorrectanswers: undefined,
      correctanswer: undefined
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
    },
    //Shuffle utilizado para randomizar as respostas
    shuffle(array) {
      array.sort(() => Math.random() - 0.5);
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
  // margin: 60px auto;
  // max-width: 960px;
  width: 100%;
  height: 100vh;

  .menu {
    background: linear-gradient(#5896d3, rgb(182,153, 153));
    border-radius: 4px;
    padding-top: 20px;

    
    .btnEnviar{
      box-shadow: rgba(0, 0, 0, 0.5);
      border: none;
      width: 150px;
      height: 30px;
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
