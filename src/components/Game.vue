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
                <div class="card-footer">
                    <span class="starting-player" v-if="gameConfig.starting_player_id == 1"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                          <div class="legs">
                            <span class="leg-count">{{ player_one.leg }}</span>
                        </div>
                    <span class="current-player" v-if="gameConfig.current_player_id == 1"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                </div>
            </div>

            <div id="player-card" :class="gameConfig.current_player_id == 2 ? 'active-card' : ''">
                <div class="big-number" id="count2">{{ player_two.score }}</div>
                <span class="player-nickname">{{ player_two.nickname }}</span>
                <div class="card-footer">
                    <span class="starting-player" v-if="gameConfig.starting_player_id == 2"></span>
                    <span v-else class="empty-circle">&nbsp;</span>
                    <div class="legs">
                        <span class="leg-count">{{ player_two.leg }}</span>
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
                    <span class="">3-Dart AVG </span>
                </div>
                <div class="stats-player white">
                    <span class="">{{ player_one_stats.last_dart }} </span>
                    <span class="">{{ player_one_stats.darts }} </span>
                    <span class="">{{ player_one_stats.average.toFixed(1) }} </span>
                </div>
            </div>

            <div class="stats-next">
                <div class="stats-player">
                    <span class="">Last Dart </span>
                    <span class="">Darts</span>
                    <span class="">3-Dart AVG </span>
                </div>
                <div class="stats-player white">
                    <span class="">{{ player_two_stats.last_dart }}</span>
                    <span class="">{{ player_two_stats.darts }} </span>
                    <span class="">{{ player_two_stats.average.toFixed(1) }} </span>
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
                <button type="button" class="num-button orange" @click="openStats()">
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

    <div id="score_table" class="overlay">
        <a href="javascript:void(0)" class="closebtn" @click="closeStats()">&times;</a>
        <div class="wrapper">
        <div class="table-container">
            
            <span class="score_title">Statistics</span>

            <table>
                <th colspan="3"> {{ gameConfig.gamemode }}</th>
                <tr>
                    <td>Player</td>
                    <td>{{ player_one.name }}</td>
                    <td>{{ player_two.name }}</td>
                </tr>
                <tr>
                    <td>Legs</td>
                    <td>{{ player_one.leg }}</td>
                    <td>{{ player_two.leg }}</td>
                </tr>
                <th colspan="3"> Averages</th>
                <tr>
                    <td>3-Dart AVG</td>
                    <td>{{ player_one_stats.average.toFixed(1) }}</td>
                    <td>{{ player_two_stats.average.toFixed(1) }}</td>
                </tr>
                 <tr>
                    <td>Highest Dart</td>
                    <td>{{ player_one_stats.highest_dart }}</td>
                    <td>{{ player_two_stats.highest_dart }}</td>
                </tr>
                <th colspan="3"> Checkouts</th>
                <tr>
                    <td>Highest finish</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>Checkoutrate %</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>Best leg</td>
                    <td></td>
                    <td></td>
                </tr>
                <tr>
                    <td>Worst leg</td>
                    <td></td>
                    <td></td>
                </tr>
                <th colspan="3"> Highscores</th>
                <tr>
                    <td>180</td>
                    <td>{{ player_one_stats.onehundredeighties }}</td>
                    <td>{{ player_two_stats.onehundredeighties }}</td>
                </tr>
                <tr>
                    <td>160+</td>
                    <td>{{ player_one_stats.onehundredsixties}}</td>
                    <td>{{ player_two_stats.onehundredsixties }}</td>
                </tr>
                <tr>
                    <td>140+</td>
                    <td>{{ player_one_stats.onehundredfourties }}</td>
                    <td>{{ player_two_stats.onehundredfourties }}</td>
                </tr>
                <tr>
                    <td>120+</td>
                    <td>{{ player_one_stats.onehundredtwenties }}</td>
                    <td>{{ player_two_stats.onehundredtwenties }}</td>
                </tr>
                <tr>
                    <td>100+</td>
                    <td>{{ player_one_stats.onehundreds }}</td>
                    <td>{{ player_two_stats.onehundreds}}</td>
                </tr>
                <tr>
                    <td>80+</td>
                    <td>{{ player_one_stats.eighties}}</td>
                    <td>{{ player_two_stats.eighties }}</td>
                </tr>
            </table>
           <!--  <br>
            <p>Gamelog:</p>
            <div v-for="entry in score_table.slice(1)">
               {{ entry.round }},  Player: {{ entry.player_id }} {{ entry.name }}, Score: {{ entry.score }},  Score Thrown: {{ entry.score_thrown }}  
            </div>
 -->
        </div>
        </div>
    </div> 

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
    leg: number
    statistics: Stats
}

interface Stats {
    darts: number
    last_dart: number
    highfinish: number
    highest_dart: number
    onehundredeighties: number
    onehundredsixties: number
    onehundredfourties: number
    onehundredtwenties: number
    onehundreds: number
    eighties: number
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
    leg: number;
    stats: Statistics

    constructor(id: number, name: string, nickname: string, score: number, leg: number, stats: Statistics) {
        this.id = id
        this.name = name
        this.nickname = nickname
        this.score = score
        this.leg = leg
        this.stats = stats
    }
}

class Statistics {
    darts: number;
    last_dart: number;
    highfinish: number;
    highest_dart: number;
    onehundredeighties: number;
    average: number;
    checkout_ratio: number;
    onehundredsixties: number;
    onehundredfourties: number;
    onehundredtwenties: number;
    onehundreds: number;
    eighties: number;

    constructor(darts: number, last_dart: number, highfinish: number, onehundredeighties: number, highest_dart: number, average: number, checkout_ratio: number,
     onehundredsixties: number,
    onehundredfourties: number,
    onehundredtwenties: number,
    onehundreds: number,
    eighties: number) {
        this.darts = darts
        this.last_dart = last_dart
        this.highfinish = highfinish;
        this.highest_dart = highest_dart;
        this.onehundredeighties = onehundredeighties;
        this.onehundredsixties = onehundredsixties;
        this.onehundredfourties = onehundredfourties;
        this.onehundredtwenties = onehundredtwenties;
        this.onehundreds = onehundreds;
        this.eighties = eighties;
        this.average = average;
        this.checkout_ratio = checkout_ratio;
    }
}

let player_one = ref(<Player>{})
let player_two = ref(<Player>{})
let player_one_stats = ref(<Statistics>{})
let player_two_stats = ref(<Statistics>{})


let gameConfig = ref(props.config)

interface checkoutType{
    [key: number]: string;
}

let _checkoutData: checkoutType = checkoutTable()

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

        // stats
        let _dart = Number(score_input.value) 

        if (_dart >= player_one_stats.value.highest_dart) player_one_stats.value.highest_dart = _dart

        if (_dart == 180) player_one_stats.value.onehundredeighties++
        if (_dart >= 160 && _dart != 180) player_one_stats.value.onehundredsixties++
        if (_dart >= 140 && _dart < 160 && _dart != 180) player_one_stats.value.onehundredfourties++
        if (_dart >= 120 && _dart < 140 && _dart != 180) player_one_stats.value.onehundredtwenties++
        if (_dart >= 100 && _dart < 120 && _dart != 180) player_one_stats.value.onehundreds++
        if (_dart >= 80 && _dart < 100 && _dart != 180) player_one_stats.value.eighties++  


        if (_temp_score > 0) {

            if (_temp_score !== player_one.value.score) {
                await counter("count1", player_one.value.score, _temp_score, 500);
            }

            player_one.value.score = _temp_score

            score_table.value.push({
                round: -Math.round(-gameConfig.value.round),
                score: player_one.value.score,
                player_id: player_one.value.id,
                score_thrown: Number(score_input.value),
                name: player_one.value.name
            })

            // stats
            player_one_stats.value.average = Number(getCurrentAvg(player_one.value.id))
            player_one_stats.value.last_dart = Number(score_input.value);
            player_one_stats.value.darts += 3;

            gameConfig.value.current_player_id = 2
            gameConfig.value.round = gameConfig.value.round + .5

        }

        // winner
        if (_temp_score == 0 && player_one.value.leg < 3) { 

            player_one.value.leg++

      /*       if (_temp_score !== player_one.value.score) {
                counter("count1", player_one.value.score, _temp_score, 500);
            } */

            player_one.value.score = 501

            score_table.value.push({
                round: -Math.round(-gameConfig.value.round),
                score: player_one.value.score,
                player_id: player_one.value.id,
                score_thrown: Number(score_input.value),
                name: player_one.value.name
            })

            alert(player_one.value.name + 'won the Leg! ' + player_one_stats.value.darts + ' Darts thrown with an 3-Dart average of ' + player_one_stats.value.average.toFixed(1))

            reset_stats()

            if (gameConfig.value.starting_player_id == 1) { 
                gameConfig.value.starting_player_id = 2
                gameConfig.value.current_player_id = 2
            }

            gameConfig.value.round = 1

        }

        if (player_one.value.leg == 3) {
            alert(player_one.value.name + 'won the Game!')
            emit('reset')
        }

    

        if (_checkoutData.hasOwnProperty(player_two.value.score)) {
            checkdarts.value  = _checkoutData[player_two.value.score] 
        }

    } else {

        // Player 2
        let _temp_score = player_two.value.score - Number(score_input.value)

        // stats
        let _dart = Number(score_input.value)

        if (_dart >= player_two_stats.value.highest_dart) player_two_stats.value.highest_dart = _dart

        if (_dart == 180) player_two_stats.value.onehundredeighties++
        if (_dart >= 160 && _dart != 180) player_two_stats.value.onehundredsixties++
        if (_dart >= 140 && _dart < 160 && _dart != 180) player_two_stats.value.onehundredfourties++
        if (_dart >= 120 && _dart < 140 && _dart != 180) player_two_stats.value.onehundredtwenties++
        if (_dart >= 100 && _dart < 120 && _dart != 180) player_two_stats.value.onehundreds++
        if (_dart >= 80 && _dart < 100 && _dart != 180) player_two_stats.value.eighties++


        if (_temp_score > 0) {

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

            // stats
            player_two_stats.value.average = Number(getCurrentAvg(player_two.value.id))
            player_two_stats.value.last_dart = Number(score_input.value);
            player_two_stats.value.darts += 3;

            gameConfig.value.current_player_id = 1
            gameConfig.value.round = gameConfig.value.round + .5
        }

        // winner
        if (_temp_score == 0 && player_two.value.leg < 3) {

            player_two.value.leg++

           /*  if (_temp_score !== player_two.value.score) {
                counter("count1", player_two.value.score, _temp_score, 500);
            } */

            player_two.value.score = 501

            score_table.value.push({
                round: -Math.round(-gameConfig.value.round),
                score: player_two.value.score,
                player_id: player_two.value.id,
                score_thrown: Number(score_input.value),
                name: player_two.value.name
            })

            alert(player_two.value.name + 'won the Leg! ' + player_two_stats.value.darts + ' Darts thrown with an 3-Dart average of ' + player_two_stats.value.average.toFixed(1))

            reset_stats()

            if (gameConfig.value.starting_player_id == 2) {
                gameConfig.value.starting_player_id = 1
                gameConfig.value.current_player_id = 1
            }
            
            gameConfig.value.round = 1

        }

        if (player_two.value.leg == 3) {
            alert(player_two.value.name + 'won the Game!')
            emit('reset')
        }

        if (_checkoutData.hasOwnProperty(player_one.value.score)) {
            checkdarts.value = _checkoutData[player_one.value.score]
        }
    }

    score_input.value = ''

}

const initGame = () => {
    player_one_stats.value = new Statistics(0, 0, 0, 0, 0, 0,0,0,0,0,0,0)
    player_two_stats.value = new Statistics(0, 0, 0, 0, 0, 0,0,0,0,0,0,0)

    // initialize players
    player_one.value = new Player(1, gameConfig.value.player_one_name, gameConfig.value.player_one_name, gameConfig.value.score, 0, player_one_stats.value)
    player_two.value = new Player(2, gameConfig.value.player_two_name, gameConfig.value.player_two_name, gameConfig.value.score, 0, player_two_stats.value)

}


initGame()


const quitGame = () => { 
   if(confirm('Are you sure to quit?')) emit('reset')
}

const emit = defineEmits(['reset'])

const reset_stats = async () => { 
    player_two.value.score = 501
    player_one.value.score = 501

    player_one_stats.value.average = 0
    player_one_stats.value.last_dart = 0;
    player_one_stats.value.darts = 0;
    player_two_stats.value.average = 0
    player_two_stats.value.last_dart = 0;
    player_two_stats.value.darts = 0;

}

const openStats = () => {
    let _el = document.getElementById("score_table") as HTMLElement;
    _el.style.display = "block";
}
const closeStats = () => {
    let _el = document.getElementById("score_table") as HTMLElement;
    _el.style.display  = "none";

}



// helper functions
const counter = async (id: string, start: number, end: number, duration: number) => {
    let obj = document.getElementById(id) as HTMLElement,
        current = start,
        range = end - start,
        increment = end > start ? 1 : -1,
        step = Math.abs(Math.floor(duration / range)),
        timer = setInterval(() => {
            current += increment;
            obj.textContent = current.toString();
            if (current == end) {
                clearInterval(timer);
            }
        }, step);
}

// calc averages
const getCurrentAvg = (player_id: number | string ) => {

    let _total = 0
    let _countItems = 0

    score_table.value.forEach(item => {
       
        if (item.player_id == player_id) {
            _countItems++
            _total += item.score_thrown
        }

    })

    _total = _total / _countItems

    return  _total
}

</script>

