<template>
  <div id="id-board">
    <div class="gameStatus" :class="gameStatusColor">{{ gameStatusMessage }}</div>
    <table class="grid">
      <tr>
        <grid-cell name="1"></grid-cell>
        <grid-cell name="2"></grid-cell>
        <grid-cell name="3"></grid-cell>
      </tr>
      <tr>
        <grid-cell name="4"></grid-cell>
        <grid-cell name="5"></grid-cell>
        <grid-cell name="6"></grid-cell>
      </tr>
      <tr>
        <grid-cell name="7"></grid-cell>
        <grid-cell name="8"></grid-cell>
        <grid-cell name="9"></grid-cell>
      </tr>
    </table>
    <button id="reset-btn" class="btn btn-success" @click="resetGame">Reset</button>
  </div>
</template>

<script>
import Cell from "./Cell.vue";

export default {
  components: {
    "grid-cell": Cell
  },
  data() {
    return {
      activePlayer: "O",
      moves: 0,
      cells: {
        1: "",
        2: "",
        3: "",
        4: "",
        5: "",
        6: "",
        7: "",
        8: "",
        9: ""
      },
      gameStatusMessage: "O's Turn",
      gameStatusColor: "statusTurn",
      winingCombination: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7]
      ]
    };
  },
  methods: {
    changePlayer() {
      this.activePlayer = this.getNonActivePlayer;
    },
    changeGameStatus() {
      if ( this.isWon() ) {
        this.gameStatusMessage = this.activePlayer + ' Won';
        Event.$emit('gameWon', this.activePlayer);
        Event.$emit('gameEnd');
      }
      else if ( this.moves == 9 )
      {
        this.gameStatusMessage = 'Match Draw';
        Event.$emit('gameEnd');
      }
      else
      {
        this.gameStatusMessage = this.getNonActivePlayer + '`s Turn';
      }
    },
    resetGame() {
      Object.assign(this.$data, this.$options.data());
      Event.$emit("clearCell");
    },
    isWon() {
      let player = this.activePlayer;
      let wc     = this.winingCombination;
      for ( let i = 0 ; i < wc.length ; i++ )
      {
         if ( this.cells[wc[i][0]] == player && 
              this.cells[wc[i][1]] == player && 
              this.cells[wc[i][2]] == player )
          {
            return true;
          }
      }
      
      return false;
    }
  },
  computed: {
    getNonActivePlayer() {
      return this.activePlayer == "O" ? "X" : "O";
    }
    
  },
  created() {
    Event.$on("strike", cellNumber => {
      this.cells[cellNumber] = this.activePlayer;
      this.moves++;
      this.changeGameStatus();
      this.changePlayer();
    });
  }
};
</script>

<style>
.grid {
  margin: 0 auto;
  background-color: #34495e;
  color: #fff;
  width: 50%;
  border-collapse: collapse;
}
#reset-btn {
  margin: 0px 0px 10px 25%;
  border-bottom-right-radius: 100px;
  border-bottom-left-radius: 100px;
  width: 50%;
}
.gameStatus {
  margin: 0px 0px 10px 25%;
  padding: 15px;
  border-top-left-radius: 100px;
  border-top-right-radius: 100px;
  background-color: #f1c40f;
  color: #fff;
  font-size: 1.4em;
  text-align: center;
  font-family: sans-serif;
  width: 50%;
}
.statusTurn {
  background-color: #f1c40f;
}
.statusWin {
  background-color: #2ecc71;
}
.statusDraw {
  background-color: #9b59b6;
}
</style>