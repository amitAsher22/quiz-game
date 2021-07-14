<template>
 <div>
   <section class="score">Player <span>{{winAnswer}}</span>X<span>{{loseAnswer}}</span>Computer</section>
 </div>

</template>

<script>
export default {
  name: "Score",
  props:['winAnswer' , 'loseAnswer']

}
</script>

<style scoped>
section.score{
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;
}
section.score span{
  padding: 8px;
  border: 1px solid black;
  font-weight: bold;
}
</style>