<template>
  <div id="app">
    <input type="radio" id="player" value="Player" v-model="user" />
    <label for="player">Player</label>
    <input type="radio" id="master" value="Master" v-model="user" />
    <label for="master">Master</label>
    <div style="display: flex;">
      <div style="width: 20%;">
        <template v-for="(game,index) in buildBracket(games)">
          <div :key="index" style="display: flex;flex-direction: row;">
            <ul>
              <li>{{game.player1}}</li>
              <li>{{game.player2}}</li>
            </ul>
          </div>
        </template>
      </div>
      <bracket :gamesPlayer="games" :gamesMaster="games" :round="games.length/2" :user="user" />
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
      ],
      user: ""
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

<style>
#app {
  padding: 15px;
}
.config {
  padding-bottom: 15px;
  padding-left: 10px;
  font-size: 18px;
}
.config input {
  padding: 5px 12px;
  font-size: 16px;
}
ul {
  width: 50%;
  float: left;
  list-style-type: none;
}
ul li {
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
</style>
