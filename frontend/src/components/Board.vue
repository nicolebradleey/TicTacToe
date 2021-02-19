<template>
<div class="container">
<div class="endScreen" v-if="playerHasWon"> {{currentPlayer}} has won!
      <button @click="reset()">Play again?</button>
    </div>
    <div class="endScreen" v-if="boardIsFull">It's a draw! 
      <button @click="reset()">Play again?</button>
    </div>
  <div class="board">
    
    <div class="row" v-for="(row, rowIndex) in board" :key="rowIndex">
      <div
        class="column"
        v-for="(column, columnIndex) in row"
        :key="columnIndex"
        @click="play(columnIndex, rowIndex)"
      >
        {{ board[rowIndex][columnIndex] }}
        
      </div>
    </div>
  </div>
  </div>
</template>

<script>

export default {
  name: "board",
  data() {
    return {
      board: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      currentPlayer: "x",
      selectedColumn: null,
      selectedRow: null,
      markerPlaced: false,
      playerHasWon: false,
      boardIsFull: false,
      moves: 0
    };
  },
  methods: {
    //method to set index of selected square by sending the indexes as an argument to selectsquare method.
    play(columnIndex, rowIndex) {
      this.selectedColumn = columnIndex;
      this.selectedRow = rowIndex;
      this.placeMarker();
      this.winner()
      if(this.moves === 5 && this.playerHasWon === false){
        this.boardIsFull = true
      }
      if (this.markerPlaced === true && this.playerHasWon === false) {
        this.changePlayer();
        this.botMove();
       
      }
    },
    changePlayer() {
      //sets current player to either x or o
      this.currentPlayer = this.currentPlayer === "x" ? "o" : "x";
    },
    placeMarker() {
      //checks if there is a free space in board array AND if markerplaced is false
      if (
        this.board[this.selectedRow][this.selectedColumn] === "" &&
        this.markerPlaced === false
      ) {
        //sets the space in board array to current players marker
        this.board[this.selectedRow][this.selectedColumn] = this.currentPlayer;
        this.markerPlaced = true;
        this.moves++
        
      }
      else {
        //sets to false if selecting already filled square
        this.markerPlaced = false;
        
      }
    },
    async botMove() {
      let i = Math.floor(Math.random() * 3);
      let j = Math.floor(Math.random() * 3);
      //places o on random empty space in array after current player move
      if (this.board[i][j] == "") {
        await this.sleep(1000);
        this.board[i][j] = this.currentPlayer;
        this.winner();
        if(this.playerHasWon === false){
        this.changePlayer();
        this.markerPlaced = false;
        return;
        }
      }
      //loops through remaining free spaces in array, to not terminate after above
      else {
        for (let x = 0; x < this.board.length; x++) {
          for (let z = 0; z < this.board[x].length; z++) {
            if (this.board[x][z] == "") {
              await this.sleep(1000);
              this.board[x][z] = this.currentPlayer;
              this.winner();
              if(this.playerHasWon === false){
              this.changePlayer();
              this.markerPlaced = false
              }
              return
            }
          }
        }
      } 
    },
    sleep(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },
    winner() {
      let winningCombination = this.currentPlayer === 'x' ? 'xxx':'ooo'

      for (let x = 0; x < this.board.length; x++) {
        for (let z = 0; z < this.board[x].length; z++) {
          if (
            ("" + this.board[x][0] + this.board[x][1] + this.board[x][2]) ===   //horizontal
             winningCombination||
            ("" + this.board[0][0] + this.board[1][1] + this.board[2][2]) === //diagonally
              winningCombination ||
           ("" + this.board[2][0] + this.board[1][1] + this.board[0][2]) === //diagonally
              winningCombination||
             ("" + this.board[0][z] + this.board[1][z] + this.board[2][z]) === //vertical
              winningCombination
          ) {
           
            this.playerHasWon = true
            break
            
          }
        }
      }
    },
    reset(){
      //sends out a message to parent component (app) to re-render board 
      this.$emit('rerender')
        },
    
  
  },
};
</script>

<style scoped>
.container{
  display: inline-grid;
}
.board {
  float: center;
  display: inline-grid;
  cursor: pointer;
  color:whitesmoke;
  
}

.row {
  clear: none;
  
  
}
.column {
  border: 1px solid whitesmoke;
  width: 200px;
  height: 175px;
  float: left;
  text-align: center;
  padding-top: 15px;
  font-size: 100px; 
}
.column:hover{
background-color:rgba(245, 245, 245, 0.274);
}
.endScreen {
  display: inline-grid;
  color: aliceblue;
  text-align: center;
  font-size: 3vw;
 
}
button{
  color: white;
  text-decoration: none;
  border: none;
  font-size: 1.2vw;
  margin-bottom: 50px;
  margin-top:40px;
  cursor: pointer;
}

</style>
