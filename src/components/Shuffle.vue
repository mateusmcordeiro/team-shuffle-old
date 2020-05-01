<template>
  <div id="shuffle" class="container">
      <h1>Teams Shuffle</h1>
      <div class=""></div>
      <div id="step-1">
        <label>Team Leaders:</label>
        <input v-model="teamLeaders[0]">
        <input v-model="teamLeaders[1]">
        <button v-on:click="shuffleLeaders">Next Step</button>
      </div>
      <div id="step-2">
        <label>Numero de jogadores total:</label>
        <input v-model="playersQuantity">
        <button v-on:click="populatePlayersArray">Generate Inputs</button>
        <div class="player-list" v-if="players.length > 0">
          <div class="form-group">
            <input v-model="leadersShuffled[0]" disabled>
          </div>
          <div class="form-group">
            <input v-model="leadersShuffled[1]" disabled>
          </div>
          <div class="form-group" v-for="(player,key) in players" :key="key">
            <input v-model="players[key]">
          </div>
          <button v-on:click="shuffleTeams">Shuffle Teams</button>
        </div>
      </div>
      <div v-if="leadersShuffled.length === 2 && spliceIndex !== 0">
        <h2>Team 1 (<span>{{leadersShuffled[0]}}</span>)</h2>
        <ul>
          <li>{{leadersShuffled[0]}}</li>
          <li v-for="(player,key) in playersShuffled.slice(spliceIndex,playersShuffled.length)" :key="key">{{player}}</li>
        </ul>
        <h2>Team 2 (<span>{{leadersShuffled[1]}}</span>)</h2>
        <ul>
          <li>{{leadersShuffled[1]}}</li>
          <li v-for="(player,key) in playersShuffled.slice(0,spliceIndex)" :key="key">{{player}}</li>
        </ul>
      </div>
      <div class="teams">

      </div>

  </div>
</template>

<script>
import * as _ from 'lodash';
export default {
  name: 'Shuffle',
  data() {
    return{
      teamLeaders: [
        '', ''
      ],
      leadersShuffled:[],
      players: [

      ],
      playersShuffled: [],
      playersQuantity: 0,
      spliceIndex: 0
    }
  },
  computed: {
    isNotEqual() {
      return this.playerQuantityNoLeaders % 2 !== 0;
    },
    playerQuantityNoLeaders() {
      return this.playersQuantity - 2;
    }
  },
  methods: {
    populatePlayersArray() {
      this.resetArray();
      for(let key = 0; key < this.playersQuantity - 2; key++) {
        this.players.push('');
      }
    },
    resetArray() {
      this.players = [];
    },
    shuffleLeaders() {
      this.leadersShuffled = _.shuffle(this.teamLeaders);
    },
    generateSpliceIndex() {
      if (this.isNotEqual) {
        const factor = this.random();
        this.spliceIndex = (this.playerQuantityNoLeaders - 1) / 2;
        this.spliceIndex += factor;
      } else {
        this.spliceIndex = this.playerQuantityNoLeaders / 2;
      }
    },
    shuffleTeams() {
      this.generateSpliceIndex();
      this.playersShuffled = _.shuffle(this.players);
    },
    random() {
      return Math.floor( Math.random() * 10) < 5 ? 0 : 1;
    }
  }

}
</script>

<style lang="scss">

</style>
