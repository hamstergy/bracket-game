<template>
  <div class="recursive">
    <div class="round">
      <!-- <h3 v-if="round==1">Final</h3> -->
      <!-- <div v-if="roundWinners.length == 1">{{ roundWinners[0] }}</div> -->
      <template v-if="round>=1">
        <div>
          <template v-for="(game,index) in buildBracket()">
            <div :key="index" :class="participant()" style="display: flex;position: relative;">
              <select
                class="bracket-select"
                name="test"
                v-model="saveRoundResult[index]"
                style="width: 50%;"
                :style="[ round == 1 ? winner: winner]"
              >
                <option :value="game.player1">{{game.player1}}</option>
                <option :value="game.player2">{{game.player2}}</option>
              </select>
              <div class="connector" v-if="index%4==1 && round != 2">
                <div class="merger merger--bottom" v-if="index%2==1"></div>
                <div class="line" v-if="index%2==1"></div>
              </div>
              <div class="connector" v-if="index%4!=1 && index!=0 && index!=4 && round != 2">
                <div class="merger merger--top" v-if="index%2!=1"></div>
                <div class="line" v-if="index%4==1"></div>
              </div>
              <div
                class="connector"
                v-if="index==0 && round == 2"
                style="
                  border-bottom: 2px solid rgb(212, 212, 212);
                  right: 0;
                  top: 0;
                  width: 50%;"
              ></div>
            </div>
          </template>
        </div>
      </template>
    </div>

    <bracket
      :gamesPlayer="roundWinnersPlayer"
      :gamesMaster="roundWinnersMaster"
      v-if="round>1"
      :round="round/2"
      :user="user"
      :total="total"
    />
  </div>
</template>

<script>
import Bracket from "./Bracket";

export default {
  name: "bracket",
  components: {
    bracket: Bracket
  },
  props: ["gamesPlayer", "gamesMaster", "round", "user", "total"],
  data() {
    return {
      highlightedPlayerId: null,
      roundWinners: [],
      roundWinnersPlayer: [],
      roundWinnersMaster: [],
      winner: {
        paddingTop: "5px"
      }
    };
  },
  watch: {
    user: function() {
      if (this.user == "Player") {
        this.roundWinners = this.roundWinnersPlayer;
      } else if (this.user == "Master") {
        this.roundWinners = this.roundWinnersMaster;
      }
    }
  },
  computed: {
    saveRoundResult: {
      get() {
        if (this.user == "Player") {
          return this.roundWinnersPlayer;
        } else if (this.user == "Master") {
          return this.roundWinnersMaster;
        } else {
          return "";
        }
      },
      set(value) {
        if (this.user == "Player") {
          this.roundWinnersPlayer = value;
        } else if (this.user == "Master") {
          this.roundWinnersMaster = value;
        }
      }
    }
  },
  methods: {
    buildBracket() {
      let val = [];
      if (this.user == "Player") {
        val = this.gamesPlayer;
      } else if (this.user == "Master") {
        val = this.gamesMaster;
      }
      if (val.length < this.round * 2) {
        for (let i = 0; this.round * 2 - val.length > i; i++) {
          val.push("");
        }
      }
      let roundCreate = [];
      let count = val.length / 2;

      for (let i = 0; count > i; i++) {
        let obj = {};
        obj["player1"] = val[i * 2];
        obj["player2"] = val[i * 2 + 1];
        roundCreate.push(obj);
      }
      return roundCreate;
    },
    participant() {
      if (this.total == 16) {
        if (this.round == 8) {
          return "participant--eight";
        } else if (this.round == 4) {
          return "participant--four";
        } else {
          return "participant--two";
        }
      } else if (this.total == 8) {
        if (this.round == 4) {
          return "participant--eight";
        } else {
          return "participant--four";
        }
      } else if (this.total == 4) {
        return "participant--eight";
      }
    },
    showWinner() {
      if (this.user == "Player") {
        return this.gamesPlayer[0];
      } else if (this.user == "Master") {
        return this.gamesMaster[0];
      }
    }
  }
};
</script>

<style lang="scss" scoped>
html {
  font-size: 1rem;
  font-family: Arial, Helvetica, sans-serif;
  background: #ececec;
}
.round {
  width: 20em;
  float: left;
  height: 100%;
}
.participant {
  &--eight {
    &:nth-child(even) .bracket-select {
      margin-bottom: 4.5em;
    }
    &:nth-child(odd) .bracket-select {
      margin-top: 4.5em;
    }
  }
  &--four {
    &:nth-child(even) .bracket-select {
      margin-bottom: 11.5em;
    }
    &:nth-child(odd) .bracket-select {
      margin-top: 11.5em;
    }
  }
  &--two {
    &:nth-child(even) .bracket-select {
      margin-bottom: 25.5em;
    }
    &:nth-child(odd) .bracket-select {
      margin-top: 25.5em;
    }
  }
}
.bracket-select {
  display: block;
  font-size: 16px;
  font-family: sans-serif;
  font-weight: 700;
  color: #444;
  line-height: 1.3;
  padding: 0.6em 1.4em 0.5em 0.8em;
  width: 100%;
  max-width: 100%;
  box-sizing: border-box;
  margin: 0;
  border: 1px solid #aaa;
  box-shadow: 0 1px 0 1px rgba(0, 0, 0, 0.04);
  border-radius: 0.2em;
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  background-color: #d6d6d6;
  background: #d6d6d6;
  background-repeat: no-repeat, repeat;
  background-position: right 0.7em top 50%, 0 0;
  background-size: 0.65em auto, 100%;
  height: 2.5rem;
  &::-ms-expand {
    display: none;
  }
  &:hover {
    border-color: #888;
  }
  &:focus {
    border-color: #aaa;
    box-shadow: 0 0 1px 3px rgba(59, 153, 252, 0.7);
    box-shadow: 0 0 0 3px -moz-mac-focusring;
    color: #222;
    outline: none;
  }
  & option {
    font-weight: normal;
  }
}
.item {
  display: flex;
  flex-direction: row-reverse;
  p {
    padding: 20px;
    margin: 0;
    background-color: Beige;
  }
}
.connector {
  .merger {
    width: 25%;
    border-right: 2px solid #d4d4d4;
    position: absolute;
    right: 25%;
    &--top {
      height: 100%;
      top: 0rem;
      border-bottom: 2px solid #d4d4d4;
    }
    &--bottom {
      border-top: 2px solid #d4d4d4;
      height: 100%;
      top: 0rem;
    }
  }
  .line {
    background-color: red;
    width: 25%;
    bottom: 0;
    position: absolute;
    border-top: 2px solid #d4d4d4;
    position: absolute;
    right: 0;
  }
}
</style>