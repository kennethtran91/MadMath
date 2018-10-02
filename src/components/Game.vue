<template>
  <div class="container" id="content">
    <div id="progress-bar">
      <div id="progress-bar-change"></div>
    </div>
    <div class="content">
      <v-btn depressed v-on:click="game" id="startBtn">Click To Start</v-btn>
      <div id="equation"></div>
      <div class="btn">
        <span>
          <v-btn depressed id="btn1" style="visibility: hidden;" v-on:click="trueClicked">True</v-btn>
          <v-btn depressed id="btn2" style="visibility: hidden;" v-on:click="falseClicked">False</v-btn>
        </span>
      </div>
      <br />
      <div class="score">
        <div id="score">Score: {{ this.score }}</div>
        <div id="status"></div>
      </div>
      <button v-on:click="restart" style="visibility: hidden;" id="reset">Click To Restart</button>
    </div>
  </div>
</template>

<script>
var compareSums;
var backgroundColor = ["#F39C12", "#8E44AD", "#27AE60", "#16A085", "#7F8C8D"];
var progressBar, progressTimer, timeOut;

export default {
  name: "Game",
  data() {
    return {
      score: 0,
      state: ""
    };
  },
  methods: {
    restart() {
      this.score = 0;
      this.game();
      document.getElementById("reset").style.visibility = "hidden";
    },
    gameStart() {
      this.setBackgroundColor();
      document.getElementById("startBtn").style.visibility = "hidden";
      document.getElementById("btn1").style.visibility = "visible";
      document.getElementById("btn2").style.visibility = "visible";
      document.getElementById("status").innerHTML = "";
      document.getElementById("progress-bar-change").style.visibility =
        "hidden";
    },
    game() {
      this.countDownTimer();
      console.log(this.score);
      this.gameStart();
      var operands = [1, 2, 3, 4, 5, 6, 7, 8, 9];
      var deviation = [-1, 0, 2, 1, -3];
      var operand1 = operands[Math.floor(Math.random() * operands.length)];
      var operand2 = operands[Math.floor(Math.random() * operands.length)];
      var correctSum = operand1 + operand2;

      var shownSum =
        correctSum + deviation[Math.floor(Math.random() * deviation.length)];

      var resultString =
        operand1.toString() +
        " + " +
        operand2.toString() +
        "<br /> = " +
        shownSum.toString();

      document.getElementById("equation").innerHTML = resultString;
      compareSums = correctSum == shownSum;
      console.log(compareSums);
    },

    gameOver: function() {
      document.getElementById("status").innerHTML = "Game Over";
      document.getElementById("reset").style.visibility = "visible";
      document.getElementById("btn1").style.visibility = "hidden";
      document.getElementById("btn2").style.visibility = "hidden";
    },

    trueClicked: function() {
      if (compareSums == false) {
        this.gameOver();
      } else {
        this.game();
        this.score++;
      }
    },
    falseClicked: function() {
      if (compareSums == true) {
        this.gameOver();
      } else {
        this.game();
        this.score++;
      }
    },
    setBackgroundColor: function() {
      var color =
        backgroundColor[Math.floor(Math.random() * backgroundColor.length)];
      document.getElementById("content").style.backgroundColor = color;
    },
    countDownTimer: function() {
      progressBar = document.getElementById("progress-bar-change");
      var width = 10;
      var id = setInterval(frame, 10);
      function frame() {
        if (width >= 100) {
          clearInterval(id);
        } else {
          width++;
        }
      }
    }
  }
}
</script>

<style scoped>
body {
  display: fixed;
  height: 655px;
  overflow-y: scroll;
  overflow-x: hidden; /* hides the horizontal scroll bar */
}
.container {
  width: 500px;
  height: 655px;
  margin-top: 10px;
  margin-left: auto;
  margin-right: auto;
  border-left: 3px solid #ffffff;
  border-right: 3px solid #ffffff;
  border-bottom: 3px solid #ffffff;
  border-radius: 8px;
  background-color: cornflowerblue;
}
#equation {
  height: 53%;
  color: #ffffff;
  font-size: 120px;
  text-align: center;
}

#btn1 {
  background-color: green;
  color: #ffffff;
}
#btn2 {
  background-color: red;
  color: #ffffff;
}
#progress-bar-change {
  height: 10px;
  background-color: #4caf50;
  text-align: center; /* To center it horizontally (if you want) */
  line-height: 10px; /* To center it vertically */
  color: white;
}
.score {
  font-size: 32pt;
  text-transform: uppercase;
  color: #ffffff;
}
.content {
  align-content: center;
}
</style>
