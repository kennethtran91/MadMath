<template>
  <div class="container" id="content">
    <div id="progress-bar">
      <div id="bar" v-bind:style="{ width: value + '%'}">
    </div>
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
          <v-btn depressed x-large id="btn1" style="visibility: hidden;" v-on:click="trueClicked"><v-icon>done</v-icon></v-btn>
          <v-btn depressed x-large id="btn2" style="visibility: hidden;" v-on:click="falseClicked"><v-icon>clear</v-icon></v-btn>
        </span>
      </div>
      <div class="status">
        <div id="blinking" style="visibility: hidden; color: #DC4C46" v-on:click="restart">GAME OVER</div>
      </div>
    </div>
  </div>
</template>

<script>
var compareSums;
var backgroundColor = [
  "#F39C12",
  "#8E44AD",
  "#27AE60",
  "#16A085",
  "#7F8C8D",
  "#006E6D",
  "#482e51"
];
var timeInterval;

export default {
  name: "Game",
  data() {
    return {
      score: 0,
      value: 0
    };
  },
  // beforeDestroy () {
  //     clearInterval(this.timeInterval)
  //   },
  methods: {
    restart() {
      document.getElementById("blinking").style.visibility = "hidden";
      this.score = 0;
      this.game();
    },
    gameStart() {
      this.setBackgroundColor();
      document.getElementById("startBtn").style.visibility = "hidden";
      document.getElementById("btn1").style.visibility = "visible";
      document.getElementById("btn2").style.visibility = "visible";
      document.getElementById("score").style.visibility = "visible";
      document.getElementById("bar").style.visibility = "visible";
      document.getElementById("reset").style.visibility = "visible";
    },
    game() {
      this.timeReset();
      // console.log(this.score);
      this.gameStart();
      //var operands = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 13, 15, 20, 92, 99];
      var operands = Array.from(
        { length: 15 },
        () => Math.floor(Math.random() * 20) + 1
      );
      var deviation = [-1, 0, 1];
      var operand1 = operands[Math.floor(Math.random() * operands.length)];
      var operand2 = operands[Math.floor(Math.random() * operands.length)];
      var correctSum = operand1 + operand2;

      var shownSum = correctSum + deviation[Math.floor(Math.random() * deviation.length)];

      var resultString =
        operand1.toString() +
        " + " +
        operand2.toString() +
        "<br /> = " +
        shownSum.toString();

      document.getElementById("equation").innerHTML = resultString;
      compareSums = correctSum == shownSum;
      // console.log(compareSums);
    },

    gameOver: function() {
      clearInterval(this.timeInterval);
      document.getElementById("blinking").style.visibility = "visible";
      document.getElementById("reset").style.visibility = "visible";
      document.getElementById("btn1").style.visibility = "hidden";
      document.getElementById("btn2").style.visibility = "hidden";
      document.getElementById("bar").style.visibility = "hidden";
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
      var color =
        backgroundColor[Math.floor(Math.random() * backgroundColor.length)];
      document.getElementById("content").style.backgroundColor = color;
    },
    timeReset: function() {
      this.value = 100;
      clearInterval(timeInterval);
      var timeInterval = setInterval(() => {
        this.value--;
      }, 150);
    }
  },
  watch: {
    value: function(event) {
      if (this.value == 0) {
        clearTimeout(timeInterval);
        this.gameOver();
      }
    }
  }
};
</script>

<style scoped>
body {
  display: fixed;
  overflow-y: hidden;
  overflow-x: hidden; /* hides the horizontal scroll bar */
}
.container {
  margin-top: 1px;
  margin-left: auto;
  margin-right: auto;
  border-left: 3px solid #ffffff;
  border-right: 3px solid #ffffff;
  border-bottom: 3px solid #ffffff;
  border-radius: 8px;
  background-color: cornflowerblue;
  min-height: 100vh;
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
#score,
#resetBtn {
  display: inline;
}
#equation {
  height: 53%;
  color: #ffffff;
  font-size: 5.5em;
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
  background-color: #4caf50;
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
#startBtn {
  background-color: #9068be;
  color: white;
  font-size: 20px;
  border-radius: 2px;
  box-shadow: 0 9px gray;
  border-radius: 18px;
}
#startBtn:hover {
  background-color: #dddfd4;
  color: white;
  font-size: 20px;
  border-radius: 2px;
  box-shadow: 0 9px #999;
  border-radius: 18px;
}
#bar {
  height: 10px;
  background: darkred;
  display: block;
  width: 100%;
  border-radius: 12px;
}
#score {
  font-size: 11pt;
  text-transform: uppercase;
  color: #ffffff;
}
.status {
  font-size: 4em;
  text-transform: uppercase;
  letter-spacing: 3px;
}
.content {
  height: 100%;
}
@keyframes blinking {
  0% {
    opacity: 0;
    letter-spacing: 1px;
  }
  50% {
    opacity: 0.5;
    letter-spacing: 2px;
  }
  100% {
    opacity: 1;
    letter-spacing: 3px;
  }
}

#blinking {
  animation: blinking 1s infinite;
}
</style>
