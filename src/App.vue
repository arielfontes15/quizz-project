<template>
  <div class="menu">
    <h1>Mamãe no céu tem pão?</h1>
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
  components: {
  }
}

//https://opentdb.com/api.php?amount=10&category=18
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #0b0d0f;
  margin: 60px auto;
  max-width: 960px;

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
