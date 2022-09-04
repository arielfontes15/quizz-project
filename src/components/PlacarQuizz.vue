<template>
  <div class="placar">
      <span class="itensPlacar">Correct Answers: {{this.countCorrect}}</span>
        💥
      <span class="itensPlacar">Incorrect Answers: {{this.countIncorrect}}</span>
    </div>
</template>

<script>
export default {
    name: 'PlacarQuizz',
    props: ["countCorrect", "countIncorrect"],
    data(){
        return {

        }
    }
}
</script>

<style>

</style>