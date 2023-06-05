<template>
  <div>
    <div class="gameStatus" :class="gameStatusColor">
      {{ gameStatusMessage }}
    </div>
    <table class="grid">
      <tr>
        <CellVue name="1"></CellVue>
        <CellVue name="2"></CellVue>
        <CellVue name="3"></CellVue>
      </tr>
      <tr>
        <CellVue name="4"></CellVue>
        <CellVue name="5"></CellVue>
        <CellVue name="6"></CellVue>
      </tr>
      <tr>
        <CellVue name="7"></CellVue>
        <CellVue name="8"></CellVue>
        <CellVue name="9"></CellVue>
      </tr>
    </table>
  </div>
</template>

<script>
import CellVue from "./TheCells.vue";
import { emitter } from "@/main";

export default {
  components: { CellVue },

  data() {
    return {
      activePlayer: "X",
      gameStatus: "turn",
      gameStatusMessage: `Ходит: X`,
      gameStatusColor: "statusTurn",
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
        9: "",
      },
      winConditions: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7], 
      ],
    };
  },

  computed: {
    nonActivePlayer() {
      if (this.activePlayer === "O") {
        return "X";
      }

      return "O";
    },
  },

  watch: {
    gameStatus() {
      if (this.gameStatus === "win") {
        this.gameStatusColor = "statusWin";

        return;
      } else if (this.gameStatus === "draw") {
        this.gameStatusColor = "statusDraw";

        this.gameStatusMessage = "Ничья!";

        return;
      }

      this.gameStatusMessage = `Ходит: ${this.activePlayer}`;
    },
  },

  methods: {
    changePlayer() {
      this.activePlayer = this.nonActivePlayer;
      if(!this.checkForWin())
      this.gameStatusMessage = `Ходит: ${this.activePlayer}`;
    },

    checkForWin() {
      for (let i = 0; i < this.winConditions.length; i++) {
        let wc = this.winConditions[i];
        let cells = this.cells;

        if (this.areEqual(cells[wc[0]], cells[wc[1]], cells[wc[2]])) {
          return true;
        }
      }

      return false;
    },

    gameIsWon() {
      this.gameStatusMessage = `${this.activePlayer} победил!`;

      emitter.emit("freeze");

      return "win";
    },


    changeGameStatus() {
      if (this.checkForWin()) {
        return this.gameIsWon();
      } else if (this.moves === 9) {
        return "draw";
      }
      return "turn";
    },


    areEqual() {
      var len = arguments.length;

      for (var i = 1; i < len; i++) {
        if (arguments[i] === "" || arguments[i] !== arguments[i - 1])
          return false;
      }
      return true;
    },
  },

  created() {
    emitter.on("strike", (cellNumber) => {
      this.cells[cellNumber] = this.activePlayer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      this.changePlayer();
    });


    emitter.on("gridReset", () => {
      Object.assign(this.$data, this.$options.data.apply(this))
    });
  },
};
</script>

<style>
.grid {
  background-color: #34495e;
  color: #fff;
  width: 100%;
  border-collapse: collapse;
}

.gameStatus {
  margin: 0px;
  padding: 15px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  background-color: #f1c40f;
  color: #fff;
  font-size: 1.4em;
  font-weight: bold;
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
