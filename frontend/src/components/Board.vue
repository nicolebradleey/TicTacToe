<template>
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
      markerPlaced: false
    };
  },
  methods: {
   
    //method to set index of selected square by sending the indexes as an argument to selectsquare method.
    play(columnIndex, rowIndex) {
      this.selectedColumn = columnIndex;
      this.selectedRow = rowIndex;
      console.log(this.selectedRow, this.selectedColumn);
      this.placeMarker();
      //check winningscombos method
      if(this.markerPlaced === true){
      this.changePlayer();
      this.botMove();
      //check winningcombos
      }
    },
    changePlayer(){
      //sets current player to either x or o
      this.currentPlayer = this.currentPlayer === 'x'? 'o':'x'
      console.log(this.currentPlayer)
    },
    placeMarker() {
      //checks if there is a free space in board array AND if markerplaced is false
      if (this.board[this.selectedRow][this.selectedColumn] === "" && this.markerPlaced === false) {  
        //sets the space in board array to current players marker
        this.board[this.selectedRow][this.selectedColumn] = this.currentPlayer;
        this.markerPlaced = true
      }else{
        //sets to false if selecting already filled square
          this.markerPlaced = false
      }
    },
    async botMove() {
      let i = Math.floor(Math.random() * 3);
      let j = Math.floor(Math.random() * 3);
      //places o on random empty space in array after current player move
      if (this.board[i][j] == "") {
        await this.sleep(1000)
        this.board[i][j] = this.currentPlayer;
        this.changePlayer()
        this.markerPlaced = false
        return;
      } 
      //loops through remaining free spaces in array, to not terminate after above
      else {
        for (let x = 0; x < this.board.length; x++) {
          for (let z = 0; z < this.board[x].length; z++) {
            if (this.board[x][z] == "") {
              await this.sleep(1000)
              this.board[x][z] = this.currentPlayer;
              this.changePlayer()
              this.markerPlaced = false
              return
            }
          }
        }
      }

    },
  sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
},
winner(){
//loopa igenom allt igen som i botmove. if this.board[i][j]
}
  },
};
</script>

<style scoped>
.row {
  clear: both;
}
.column {
  border: 1px solid rgba(0, 0, 0, 0.685);
  width: 200px;
  height: 175px;
  float: left;
  text-align: center;
  padding-top: 15px;
  font-size: 100px;
}
</style>
