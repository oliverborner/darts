<template>
    <div id="container">

        <section id="header">
            <span class="title">DARTS</span>
            <span class="game-mode">{{ gameConfig.gamemode }}</span>
            <span class="close" @click="quitGame()">x</span>
        </section>

        <section id="players">
            <div id="player-card" :class="gameConfig.current_player_id == 1 ? 'active-card' : ''">
                <div class="big-number" id="count1">{{ player_one.score }}</div>
                <span class="player-nickname">{{ player_one.nickname }}</span>
                <!-- <span class="player-name">{{ player_one.name }}</span> -->
                <div class="card-footer">
                    <span class="starting-player" v-if="gameConfig.starting_player_id == 1"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                          <div class="legs">
                       
                            <span class="leg-count">0</span>
                        </div>
                    <span class="current-player" v-if="gameConfig.current_player_id == 1"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                </div>
            </div>
            <div id="player-card" :class="gameConfig.current_player_id == 2 ? 'active-card' : ''">
                <div class="big-number" id="count2">{{ player_two.score }}</div>
                <span class="player-nickname">{{ player_two.nickname }}</span>
                <!-- <span class="player-name">{{ player_two.name }}</span> -->
                <div class="card-footer">
                    <span class="starting-player" v-if="gameConfig.starting_player_id == 2"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                    <div class="legs">
                      
                        <span class="leg-count">0</span>
                    </div>
                    <span class="current-player" v-if="gameConfig.current_player_id == 2"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                </div>
            </div>
        </section>

        <section id="stats">
            <div class="stats-next">
                <div class="stats-player">
                    <span class="">Last Dart </span>
                    <span class="">Darts</span>
                    <span class="">3-Dart-AVG </span>
                </div>
                <div class="stats-player white">
                    <span class="">120 </span>
                    <span class="">6</span>
                    <span class="">80,3%</span>
                </div>
            </div>

            <div class="stats-next">
                <div class="stats-player">
                    <span class="">Last Dart </span>
                    <span class="">Darts</span>
                    <span class="">3-Dart-AVG </span>
                </div>
                <div class="stats-player white">
                        <span class="">0 </span>
                        <span class="">0</span>
                        <span class="">0</span>
                </div>  
            </div>
        </section>

        <section id="infos">
            <span class="checkout" :class="checkdarts ? 'slide-bottom' : ' '">{{ checkdarts }}</span>
            <input type="text" disabled id="score" v-model="score_input" maxlength="3">
        </section>

        <section class="controls">
            <div class="numpad">
                <button type="button" class="num-button" @click="numPadInput(nums)" v-for="nums in numpad">
                    {{ nums }}
                </button>
            </div>

            <div class="controls-right">
                <button type="button" class="num-button orange" @click="score_input = ''">
                    S
                </button>
                <button type="button" class="num-button light" @click="score_input = ''">
                    R
                </button>
                <button type="button" class="num-button light" @click="score_input = ''">
                    C
                </button>
            </div>
        </section>

        <div class="bottom-buttons">
            <button type="button" class="num-button-large" @click="numPadInput(0)">
                0
            </button>
            <button type="button" class="submit-large" @click="score_input ? submit_score() : ''">
                &#8594;
            </button>
        </div>


        <!--    <div id="player_one">{{ player_one }}</div>
    <div id="player_two">{{ player_two }}</div>
 -->

      
<!--     <p>score_table</p>
    <ul>
        <li v-for="entry in score_table.slice(1)">
            Round: {{ entry.round }},  Player: {{ entry.player_id }} {{ entry.name }}, Score: {{ entry.score }},  Score Thrown: {{ entry.score_thrown }}  
        </li>
    </ul>  -->
    </div>
</template>


<script setup lang="ts">
import { ref } from "vue"
import { checkoutTable } from '@/CheckoutTable.js'

interface Player {
    id: number
    name: string
    nickname: string
    score: number
    level: number
    statistics: Stats
}

interface Stats {
    highfinish: number
    onehundredeighties: number
    average: number
    checkout_ratio: number
    // >=120, >=140, >=100
}

interface ScoreTable {
    round: number
    score: number
    player_id: number
    score_thrown: number
    name: string
}

interface GameData {
    gamemode: string,
    score: number,
    round: number,
    current_player_id: number,
    starting_player_id: number,
    player_one_name: string,
    player_two_name: string
}

interface Props {
    config: GameData
}

const props = withDefaults(defineProps<Props>(), {

})

class Player {
    id: number;
    name: string;
    nickname: string;
    score: number;
    level: number;
    stats: Statistics

    constructor(id: number, name: string, nickname: string, score: number, level: number, stats: Statistics) {
        this.id = id
        this.name = name
        this.nickname = nickname
        this.score = score
        this.level = level
        this.stats = stats
    }
}

class Statistics {
    highfinish: number;
    onehundredeighties: number;
    average: number;
    checkout_ratio: number;

    constructor(highfinish: number, onehundredeighties: number, average: number, checkout_ratio: number) {
        this.highfinish = highfinish;
        this.onehundredeighties = onehundredeighties;
        this.average = average;
        this.checkout_ratio = checkout_ratio;
    }
}

let player_one = ref(<Player>{})
let player_two = ref(<Player>{})
let player_one_stats = ref(<Statistics>{})
let player_two_stats = ref(<Statistics>{})


let gameConfig = ref(props.config)


let _checkoutData = checkoutTable()
let checkdarts = ref('')


let numpad = {
    button1: 7,
    button2: 8,
    button3: 9,
    button4: 4,
    button5: 5,
    button6: 6,
    button7: 1,
    button8: 2,
    button9: 3,
}

let score_input = ref('')

// stack, history implementation
let score_table = ref([<ScoreTable>{}])

const numPadInput = (nums: number) => {
    if (score_input.value.length < 3 &&
        Number(score_input.value + nums.toString()) < 181) {
        score_input.value = score_input.value + nums.toString()
    }

}


const submit_score = async () => {

    let _active_player = gameConfig.value.current_player_id

    checkdarts.value = ''

    // Player 1
    if (_active_player == player_one.value.id) {

        let _temp_score = player_one.value.score - Number(score_input.value)

        if (_temp_score !== player_one.value.score) {
            await counter("count1", player_one.value.score, _temp_score, 500);
        }

        player_one.value.score = _temp_score

        score_table.value.push({
            round: gameConfig.value.round,
            score: player_one.value.score,
            player_id: player_one.value.id,
            score_thrown: Number(score_input.value),
            name: player_one.value.name
        })

        gameConfig.value.current_player_id = 2
        gameConfig.value.round = gameConfig.value.round + .5

        if (_checkoutData.hasOwnProperty(player_two.value.score)) {
            checkdarts.value = _checkoutData[player_two.value.score]
        }

    } else {

        // Player 2
        let _temp_score = player_two.value.score - Number(score_input.value)

        if (_temp_score !== player_two.value.score) {
            await counter("count2", player_two.value.score, _temp_score, 500);
        }

        player_two.value.score = _temp_score

        score_table.value.push({
            round: -Math.round(-gameConfig.value.round), // negotiate hack to round down .5
            score: player_two.value.score,
            player_id: player_two.value.id,
            score_thrown: Number(score_input.value),
            name: player_two.value.name
        })

        gameConfig.value.current_player_id = 1
        gameConfig.value.round = gameConfig.value.round + .5

        if (_checkoutData.hasOwnProperty(player_one.value.score)) {
            checkdarts.value = _checkoutData[player_one.value.score]
        }
    }

    score_input.value = ''

}

const initGame = () => {
    player_one_stats.value = new Statistics(0, 0, 0, 0)
    player_two_stats.value = new Statistics(0, 0, 0, 0)

    // initialize players
    player_one.value = new Player(1, gameConfig.value.player_one_name, gameConfig.value.player_one_name, gameConfig.value.score, 0, player_one_stats.value)
    player_two.value = new Player(2, gameConfig.value.player_two_name, gameConfig.value.player_two_name, gameConfig.value.score, 0, player_two_stats.value)

}


initGame()


const quitGame = () => { 
   if(confirm('Are you sure to quit?')) emit('reset')
}

const emit = defineEmits(['reset'])




// helper functions
const counter = async (id: string, start: number, end: number, duration: number) => {
    let obj = document.getElementById(id) as HTMLElement,
        current = start,
        range = end - start,
        increment = end > start ? 1 : -1,
        step = Math.abs(Math.floor(duration / range)),
        timer = setInterval(() => {
            current += increment;
            obj.textContent = current;
            if (current == end) {
                clearInterval(timer);
            }
        }, step);
}



</script>

