<template>
  

<div>
 <h1>Tic Tac Toe</h1>

<Board @rerender="keyUpdate" :key="keyCounter" /> 
</template>

<script>
import Board from './components/Board.vue'


export default {
  name: 'App',
  data(){
    return{
      keyCounter: 0
    }
  },
  components: {
    Board
  },
  methods: {
    keyUpdate() {
      this.keyCounter++  //when key is changed it automatically re-renders the component, changes upon reset() in board
    }
  }
}
</script>

<style scoped>

h1{
  color: whitesmoke;
  display:ruby-text;
}

</style>
