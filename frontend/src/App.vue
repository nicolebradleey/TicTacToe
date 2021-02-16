<template>
<div>
 <h1>Tic Tac Toe</h1>
 <Player />
<Board /> 
</template>

<script>
import Board from './components/Board.vue'
import Player from './components/Player.vue'
import Square from './components/Square.vue'

export default {
  name: 'App',
  components: {
    Board,
    Player,
    Square
  }
}
</script>

<style scoped>

h1{
  color: black;
}

</style>
