<template>
  <div id="app">
    <input type="radio" id="player" value="Player" v-model="user" />
    <label for="player">Player</label>
    <input type="radio" id="master" value="Master" v-model="user" />
    <label for="master">Master</label>
    <div style="display: flex;padding-top: 2rem;">
      <div class="round">
        <template v-for="(game,index) in buildBracket(games)">
          <div class="matchup" :key="index">
            <ul class="match">
              <li>{{game.player1}}</li>
              <li>{{game.player2}}</li>
            </ul>
            <div class="connector">
              <div class="merger merger--bottom" v-if="index%2!=1"></div>
              <div class="merger merger--top" v-if="index%2==1"></div>
              <div class="line" v-if="index%2!=1"></div>
            </div>
          </div>
        </template>
      </div>
      <div style="width:80%;">
        <bracket
          :gamesPlayer="games"
          :gamesMaster="games"
          :round="games.length/2"
          :user="user"
          :total="games.length"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Bracket from "./components/Bracket";

export default {
  name: "app",
  components: {
    bracket: Bracket
  },
  data() {
    return {
      games: [
        "Titans",
        "Patriots",
        "Ravens",
        "Steelers",
        "Chiefs",
        "Chargers",
        "Bills",
        "Texans"
        // "Vikings",
        // "Saints",
        // "49er",
        // "Hawks",
        // "Packers",
        // "Lions",
        // "Hawerss",
        // "Eagles"
      ],
      user: "Player"
    };
  },
  computed: {},

  methods: {
    createRound() {
      return "";
    },
    buildBracket(value) {
      let count = value.length / 2;
      let roundCreate = [];
      for (let i = 0; count > i; i++) {
        let obj = {};
        obj["player1"] = value[i * 2];
        obj["player2"] = value[i * 2 + 1];
        roundCreate.push(obj);
      }
      return roundCreate;
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
#app {
  padding: 15px;
  min-width: 1200px;
  overflow-x: scroll;
}
.round {
  width: 20%;
}
.matchup {
  display: flex;
  position: relative;
}
.match {
  width: 50%;
  float: left;
  list-style-type: none;
  margin: 1em 0 1em 0;
  padding: 0;
  li {
    display: block;
    height: 2.5rem;
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
  }
}
.connector {
  .merger {
    width: 25%;
    border-right: 2px solid #d4d4d4;
    position: absolute;
    position: absolute;
    right: 25%;
    &--top {
      height: 3.5rem;
      top: 0rem;
      border-bottom: 2px solid #d4d4d4;
    }
    &--bottom {
      border-top: 2px solid #d4d4d4;
      height: 3.5rem;
      top: 3.5rem;
    }
  }
  .line {
    background-color: red;
    width: 25%;
    top: 7rem;
    position: absolute;
    border-top: 2px solid #d4d4d4;
    position: absolute;
    right: 0;
  }
}
</style>
