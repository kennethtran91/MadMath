<template>
  <div class="container" id="content">
    <div id="progress-bar" style="visibility: hidden;">
      <v-progress-linear 
      :value="value"
      background-color="blue-grey"
      color="lime"
      height="10"
      style="border-radius: 12px"
      >
      {{ value }}
    </v-progress-linear >
</div>
    <div class="top">
      <div id="score" style="visibility: hidden;">Score {{ this.score }}</div>
      <div id="resetBtn"><v-btn id="reset" v-on:click="restart" icon color="success" style="visibility: hidden;"><v-icon>cached</v-icon></v-btn></div>
    </div>
    
    <div class="content">
      <v-btn depressed large v-on:click="game" id="startBtn">Click To Start</v-btn>
      <div id="equation"></div>
      <div class="btn">
        <span>
          <v-btn depressed large id="btn1" style="visibility: hidden;" v-on:click="trueClicked"><v-icon>done</v-icon></v-btn>
          <v-btn depressed large id="btn2" style="visibility: hidden;" v-on:click="falseClicked"><v-icon>clear</v-icon></v-btn>
        </span>
      </div>
      <div class="status">
        <div id="status"></div>
      </div>
      <v-btn depressed class="btn" v-on:click="restart" style="visibility: hidden;" id="reset">Click To Restart</v-btn>
    </div>
  </div>
</template>

<script>
var compareSums;
var backgroundColor = ["#F39C12", "#8E44AD", "#27AE60", "#16A085", "#7F8C8D", "#006E6D", "#BE9EC9"];
var progressBar, progressTimer, timeOut, outOfTime;

export default {
  name: "Game",
  data() {
    return {
      score: 0,
      interval: {},
      value: 0
    };
  },
  beforeDestroy () {
      clearInterval(this.interval)
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
      document.getElementById("score").style.visibility = "visible"; 
      document.getElementById("progress-bar").style.visibility = "visible"; 
      document.getElementById("reset").style.visibility = "visible";
      document.getElementById("status").innerHTML = "";
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
      document.getElementById("status").style.color = "#DC4C46";
      document.getElementById("reset").style.visibility = "visible";
      document.getElementById("btn1").style.visibility = "hidden";
      document.getElementById("btn2").style.visibility = "hidden";
      this.value = 0;
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
      var color = backgroundColor[Math.floor(Math.random() * backgroundColor.length)];
      document.getElementById("content").style.backgroundColor = color;
    },
    countDownTimer: function() {
      this.interval = setInterval(() => {
        if (this.value == 100) {
          clearInterval(this.interval)
          this.gameOver();
        } else {
        this.value ++;
      }}, 20);
    },
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
.top {
  position: relative;
}
#score {
  position: absolute;
  right: 0;
  padding-top: 20px;
  padding-right: 15px;
}
#resetBtn {
  position: absolute;
  left: 0;
  font-size: 8px;
  padding-top: 5px;
}
#score, #resetBtn {
    display: inline;
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
  font-size: 20px;
  box-shadow: 0 5px #999;
  border-radius: 12px;
}
#btn1:hover {
  background-color: #4CAF50;
  color: white;
  font-size: 25px;
  box-shadow: 0 9px #999;
  border-radius: 12px;
}
#btn2 {
  background-color: red;
  color: #ffffff;
  font-size: 20px;
  box-shadow: 0 5px #999;
  border-radius: 12px;
}
#btn2:hover {
  background-color: #f44336;
  color: white;
  font-size: 25px;
  box-shadow: 0 9px #999;
  border-radius: 12px;
}
#startBtn{
  background-color: #9068be;
  color: white;
  font-size: 20px;
  border-radius: 2px;
  box-shadow: 0 9px gray;
  border-radius: 18px;
}
#startBtn:hover{
  background-color: #dddfd4;
  color: white;
  font-size: 20px;
  border-radius: 2px;
  box-shadow: 0 9px #999;
  border-radius: 18px;
}
.v-progress-linear{
  margin: auto;
}
#score {
  font-size: 11pt;
  text-transform: uppercase;
  color: #ffffff;
}
.status {
  font-size: 52pt;
  text-transform: uppercase;
  letter-spacing: 3px;
}
.content {
  display: inline-block;
}

</style>
