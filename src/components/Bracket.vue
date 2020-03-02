<template>
  <div style="width: 100%;">
    <div style="width: 20%;float:left;height: 100%;">
      <h3 v-if="round==1">Final</h3>
      <!-- <div v-if="roundWinners.length == 1">{{ roundWinners[0] }}</div> -->
      <template v-if="round>=1">
        <div>
          <template v-for="(game,index) in buildBracket()">
            <div :key="index" style="display: flex;flex-direction: row;">
              <select
                class="bracket-select"
                name="test"
                v-model="saveRoundResult[index]"
                style="width: 50%; float: right"
                :style="[index%2 == 1 ? {selectStyle}: selectStyle]"
              >
                <option :value="game.player1">{{game.player1}}</option>
                <option :value="game.player2">{{game.player2}}</option>
              </select>
            </div>
          </template>
        </div>
      </template>
      <template v-else>
        <div>{{ showWinner() }}</div>
      </template>
    </div>
    <h3 v-if="round==1">Winner</h3>

    <bracket
      :gamesPlayer="roundWinnersPlayer"
      :gamesMaster="roundWinnersMaster"
      v-if="round>=1"
      :round="round/2"
      :user="user"
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
  props: ["gamesPlayer", "gamesMaster", "round", "user"],
  data() {
    return {
      highlightedPlayerId: null,
      roundWinners: [],
      roundWinnersPlayer: [],
      roundWinnersMaster: [],
      selectStyle: { marginTop: "2em" }
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
      let count = val.length / 2;
      let roundCreate = [];
      for (let i = 0; count > i; i++) {
        let obj = {};
        obj["player1"] = val[i * 2];
        obj["player2"] = val[i * 2 + 1];
        roundCreate.push(obj);
      }
      return roundCreate;
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

<style>
html {
  font-size: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}
body {
  background: #f0f2f2;
}
.vtb-wrapper {
  display: flex;
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
  background-color: #fff;
  background-repeat: no-repeat, repeat;
  background-position: right 0.7em top 50%, 0 0;
  background-size: 0.65em auto, 100%;
}
.bracket-select::-ms-expand {
  display: none;
}
.bracket-select:hover {
  border-color: #888;
}
.bracket-select:focus {
  border-color: #aaa;
  box-shadow: 0 0 1px 3px rgba(59, 153, 252, 0.7);
  box-shadow: 0 0 0 3px -moz-mac-focusring;
  color: #222;
  outline: none;
}
.bracket-select option {
  font-weight: normal;
}
</style>