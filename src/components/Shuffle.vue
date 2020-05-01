<template>
  <div id="shuffle" :class="['container',nowStepClass]">
      <h1>Teams Shuffle</h1>
      <div class="content-body">
          <div class="steps">
              <div id="step-1" class="step">
                  <h2>Team Leaders</h2>
                  <div class="form-group">
                      <label>Leader 1:</label>
                      <input v-model="teamLeaders[0]">
                  </div>
                  <div class="form-group">
                      <label>Leader 2:</label>
                      <input v-model="teamLeaders[1]">
                  </div>
              </div>
              <div id="step-2" class="step">
                  <h2>How many players its gonna play?</h2>
                  <div class="form-group">
                      <input v-model="playersQuantity" type="number" min="0" max="10">
                  </div>
              </div>
              <div id="step-3" class="step">
                  <h2>Let me know all players nicks</h2>
                  <div class="player-list" v-if="players.length > 0">
                      <div class="form-group" :style="{height: `${calcStep3FormGroupHeight}px`}">
                          <input v-model="leadersShuffled[0]" :style="{fontSize: `${calcStep3FormGroupHeight/1.2}px`}" disabled>
                      </div>
                      <div class="form-group" :style="{height: `${calcStep3FormGroupHeight}px`}">
                          <input v-model="leadersShuffled[1]" :style="{fontSize: `${calcStep3FormGroupHeight/1.2}px`}" disabled>
                      </div>
                      <div class="form-group" v-for="(player,key) in players" :key="key" :style="{height: `${calcStep3FormGroupHeight}px`}">
                          <input v-model="players[key]" :style="{fontSize: `${calcStep3FormGroupHeight/1.2}px`}">
                      </div>
                  </div>
              </div>
              <div id="step-4" class="step" v-if="leadersShuffled.length === 2 && spliceIndex !== 0">
                  <div class="teams">
                      <h2>Team <span>{{leadersShuffled[0]}}</span></h2>
                      <ul>
                          <li class="leader">
                              <award-icon size="24" class="custom-class"></award-icon>
                              {{leadersShuffled[0]}}</li>
                          <li v-for="(player,key) in playersShuffled.slice(spliceIndex,playersShuffled.length)" :key="key">{{player}}</li>
                      </ul>
                      <h2>Team <span>{{leadersShuffled[1]}}</span></h2>
                      <ul>
                          <li class="leader">
                              <award-icon size="24" class="custom-class"></award-icon>
                              {{leadersShuffled[1]}}
                          </li>
                          <li v-for="(player,key) in playersShuffled.slice(0,spliceIndex)" :key="key">{{player}}</li>
                      </ul>
                  </div>
              </div>
          </div>
          <div class="buttons">
              <button v-on:click="prevStep" v-if="step > 0" class="button-icon prev">
                  <arrow-left-icon size="100"></arrow-left-icon>
              </button>
              <button v-on:click="timeOuttedShuffle" :class="[ 'button-icon','re-shuffle', reShufledClass ]" v-if="step === 4">
                  <refresh-ccw-icon size="100"></refresh-ccw-icon>
              </button>
              <button v-on:click="nextStep" v-if="step < 4" :class="['button-icon', step > 0 ? 'next' : '']">
                  {{step === 0 ? 'Start application' : ''}}
                  <arrow-right-icon size="100" v-if="step > 0"></arrow-right-icon>

              </button>
          </div>
      </div>
  </div>
</template>

<script>
import * as _ from 'lodash';
import { RefreshCcwIcon, ArrowRightIcon,ArrowLeftIcon,AwardIcon } from 'vue-feather-icons'

export default {
  name: 'Shuffle',
    components : {
        RefreshCcwIcon,
        ArrowLeftIcon,
        ArrowRightIcon,
        AwardIcon
    },
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
            spliceIndex: 0,
            step: 0,
            reshuffle: false
        }
    },
    computed: {
        nextStepTextButton() {
          return this.step === 0;
        },
        isNotEqual() {
          return this.playerQuantityNoLeaders % 2 !== 0;
        },
        playerQuantityNoLeaders() {
          return this.playersQuantity - 2;
        },
        nowStepClass() {
          return `on-step-${this.step}`;
        },
        calcStep3FormGroupHeight() {
            const height = 331.563;
            return height/this.playersQuantity;
        },
        reShufledClass() {
            return this.reshuffle ? 'clicked' : '';
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

            this.shuffleLeaders();
            this.generateSpliceIndex();
            this.playersShuffled = _.shuffle(this.players);

        },
        timeOuttedShuffle() {
            this.reshuffle = true;
            setTimeout(() => this.reshuffle = false,500);
            this.shuffleTeams();


        },
        random() {
          return Math.floor( Math.random() * 10) < 5 ? 0 : 1;
        },
        nextStep() {
          this.stepWorkFlow();
          this.step++;

        },
        prevStep() {
          this.stepWorkFlow();
          this.step--;
        },
        stepWorkFlow() {
          switch(this.step) {
            case 1:
                this.shuffleLeaders();
                break;
            case 2:
                this.populatePlayersArray();
                break;
            case 3:
                this.shuffleTeams();
                break;
            default:
                break;
          }
        }
    }

}
</script>

<style lang="scss">
    .step {
        opacity: 0;
        z-index: 1;
        h2 {
            text-align: left;
        }
    }
    #shuffle:not(.on-step-0) h1 {
        display: none;
    }
    .on-step-0  {
        .steps {
            display: none;
        }
        .buttons {
           padding-top: 0 !important;
            button {
                font-size: 26px;
                font-weight: 500;
                color: #e63946;
                border: 1px solid #e63946 !important;
                border-radius: 48px;
                padding: 14px 12px;
                transition: all .6s ease-in-out;
                &:hover,&:focus {
                    background-color: #e63946;
                    color: #f1faee;
                }
            }
        }
        .content-body > div:last-of-type {
            width: 253px !important;
        }
        .content-body .buttons > * {
            height: auto !important;
        }
        #step-0{
            opacity: 1;
            z-index: 400;
        }

    }

    .on-step-1 #step-1 {
        opacity: 1;
        z-index: 400;
        .form-group {
            height: 35%;
            input {
                font-size: 71px;
                padding-top: 15px;
            }
        }
    }
    .on-step-2 #step-2 {
        opacity: 1;
        z-index: 400;
        .form-group {
            height: 82%;
            input {
                font-size: 279px;
            }
        }
    }
    .on-step-3 #step-3 {
        opacity: 1;
        z-index: 400;
        .form-group {
            height: 58px;
            input {

            }
        }
    }

    .on-step-4 #step-4 {
        opacity: 1;
        display: flex;
        align-items: center;
    }
    .container {
        max-width: 800px;
        width: 100%;
        .content-body {
            display: flex;
            align-items: center;
            justify-content: center;
            & > div {
                height: 698px;
                width: 100%;
                &:last-of-type {
                    width: 200px;
                }
            }
            .buttons {
                display: flex;
                flex-direction: column;
                height: 307px;
                padding-top: 88px;
                & > * {
                    height: 50%;
                    background-color: transparent;
                    border: none;
                    &:focus {
                        outline: none;
                    }
                }
            }
        }
    }
    .form-group {
        display: flex;
        flex-direction: column;
        margin-bottom: 30px;
        label {
            text-align: left;
            margin-bottom: 5px;
            color: #f1faee;
            font-size: 19px;
            background-color: rgba(255,255,255,0.4);
            width: fit-content;
            padding: 10px;
            border-radius: 4px 4px 0 0px;
            margin-bottom: 0px;
            z-index: 300;
        }
        input {
            height: stretch;
            border: none;
            border-radius: 0;
            transition: border .5s ease-in-out;
            border-radius: 0 13px 13px 13px;
            background-color: rgba(255,255,255,0.4);
            box-shadow: 0px 1px 5px 1px rgba(0,0,0,.2);
            padding-left: 20px;
            color: #f1faee;
            &:focus {
                outline: none;
                border-bottom: 2px solid #a8dadc;
            }
        }
    }
    .steps {
        position: relative;
    }
    .step {
        position: absolute;
        top: 0;
        height: 100%;
        width: 100%;
        transition: all .5s ease-in-out;
    }
    .teams {
        h2 {
            font-weight: lighter;
            span {
                font-weight: bold;
            }

        }
        ul {
            list-style: none;
            padding: 0;
            width: 100%;
            li {

                color: white;
                text-align: left;
                font-size: 21px;
                font-weight: lighter;
                padding-left: 32px;
                padding-top: 4px;
                &.leader {
                    display: flex;
                    align-items: center;
                    padding-left: 4px;
                    svg {
                        padding-right: 4px;
                    }
                }
            }
        }

    }
    h1 {
        color: #f1faee;
        font-size: 51px;
    }
    h2 {
        color: #f1faee;
        font-size: 32px;
    }
    .button-icon {

        color: #e63946;
        transition: all .5s ease-in-out;
        &:hover,&:focus {
            color: #d62828;
        }
        &.next:hover {
            margin-left: 30px;
        }
        &.prev:hover {
            margin-right: 30px;
        }
        &.re-shuffle:hover {
            transform: rotate(-30deg);
        }
        &.re-shuffle.clicked {
            transform: rotate(-330deg);
        }
    }
</style>
