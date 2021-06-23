<template>
  <div id="app">
      <div class="wrapper clearfix">
        <!-- props-->
        <Players
          :isWinner="isWinner"
          :activePlayer="activePlayer"
          :scorePlayer="scorePlayer"
          :currentScore="currentScore"
        />

        <Controls
          :isPlaying="isPlaying"
          :finalScore="finalScore"
          @handleNewGame="handleNewGame"
          @handleRollDice="handleRollDice"
          @handleHoldScore="handleHoldScore"
          @handleChangeFinalScore="handleChangeFinalScore"
        />

        <Dices
        :dices="dices"
        />

        <popup-rule
          @handleConfirm="handleConfirm"
        :isOpenPopup="isOpenPopup"/>

      </div>
    </div>
</template>

<script>
import Players from "./components/Players";
import Controls from "./components/Controls";
import Dices from "./components/Dices";
import PopupRule from "./components/PopupRule";
export default {
  name: 'app',
  components: {
    Players,
    Controls,
    Dices,
    PopupRule
  },
  data () {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer: 0,
      scorePlayer: [0, 0],
      dices: [1, 1],
      currentScore: 0,
      finalScore: 10
    }
  },
  computed: {
    isWinner() {
      // return true;
      let { scorePlayer, finalScore } = this;

      if (scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore) {
        // dừng cuộc chơi
        this.isPlaying = false;
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    handleConfirm() {
      console.log("HandleConfirm App.vue");
      this.isOpenPopup = false;
      this.isPlaying = true;
      this.dices = [1, 1];
      this.currentScore = 0;
      this.scorePlayer = [0, 0];
    },
    handleNewGame() {
      console.log("handleOpenPopup App.vue");
      this.isOpenPopup = true;
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1: 0;
      this.currentScore = 0;
    },
    handleRollDice() {
      console.log("HandleRollDice App.vue");

      if (this.isPlaying) {
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;

        this.dices = [dice1, dice2];
        if (dice1 === 1 || dice2 === 1) {
          let activePlayer = this.activePlayer;
          setTimeout(function () {
            alert(`Người chơi Player ${activePlayer + 1} đã quay vào ô số 1, rất tiếc!`)
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      } else {
        alert("Vui lòng ấn new game để bắt đầu!");
      }
    },
    handleHoldScore() {
      console.log("HandleHolder App.vue");

      if (this.isPlaying) {
        // this.scorePlayer[this.activePlayer] = this.scorePlayer[this.activePlayer] + this.currentScore;
        let { scorePlayer, activePlayer, currentScore } = this;
        let scoreOld = scorePlayer[activePlayer];
        // let cloneScorePLayer = [...scoresPlayer];
        // cloneScorePLayer[activePlayer] = scoreOld + currentScore;

        this.$set(this.scorePlayer, activePlayer, scoreOld + currentScore);
        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert("Vui lòng ấn new game để bắt đầu!");
      }
    },
    handleChangeFinalScore(e) {
      // console.log(e.target.value);
      var number = parseInt(e.target.value);
      if(isNaN(number)) {
        this.finalScore = '';
      } else {
        this.finalScore = number;
      }
    }
  }
}
</script>

<style>
/**********************************************
*** GENERAL
**********************************************/

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
