<template>
  <div id="fullPage">
    <div id="scoreBoard">
        <h1>Tic Tac Toe</h1>
        <span id="player-o"><b>Player O : {{ oWin }}</b></span>
        <span id="player-x"><b>Player X : {{ xWin }}</b></span>
      </div>
      <app-board></app-board>
  </div>
</template>

<script>
import Board from './components/Board.vue';

export default { 
  components :{
    'app-board' : Board
  },
  data() {
    return {
      oWin : 0,
      xWin : 0
    };
  },
  created() {
    Event.$on('gameWon', (winner) => {
      if ( winner == 'O' )
      {
        this.oWin++;
      }
      else
      {
        this.xWin++;
      }
    })
  }
};
</script>

<style>
#fullPage {
  margin: 0% 2%;
}
#player-o{
  margin-left: 10px;
}
#player-x{
  margin-right: 10px;
}
#scoreBoard {
  margin: 10px;
  height: 90px;
  color: white;
  text-align: center;
  border-radius: 10px;
  background-color: darkolivegreen;
}
h1{
    font-family: 'Times New Roman', Times, serif;
    color: white;
}
#player-o{
    float: left;
}
#player-x{
    float: right;
}
</style>