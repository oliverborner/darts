<template>
    <div id="container">
        <section id="main-menu" v-if="!game_started">
            <section id="header">
                <span class="title">DARTS</span>
                <span class="game-mode">New Game</span>
                 <span class="close">?</span>
            </section>

            <div class="card">
                <span class="big-number">501</span>
                <span class="">Best of five</span>
            </div>

            <input type="text" class="menu-input" placeholder="Player 1" v-model="player_one_input">
            <p class="strong-grey"> VS. </p>
            <input type="text" class="menu-input" placeholder="Player 2" v-model="player_two_input">

            <button type="button" class="menu-btn" @click="startGame()">Spiel starten</button>

        </section>

        <Game v-if="game_started"
            @reset="reset"
            :config="gameConfig"/>

    </div>
</template>


<script setup lang="ts">
// Code written by Oliver Borner
// UI UX by Oliver Borner

import { ref, defineComponent } from "vue"
import Game from './components/Game.vue'


let game_started = ref(false)
let player_one_input = ref('')
let player_two_input = ref('')

let gameConfig = ref({
    gamemode: '501 - Best of 5',
    score: 501,
    round: 1,
    current_player_id: 1,
    starting_player_id: 1,
    player_one_name: '',
    player_two_name: ''
})

const reset = () => {
    game_started.value = false
}

const startGame = () => {

    if (player_one_input.value && player_two_input.value) {
        gameConfig.value.player_one_name = player_one_input.value,
        gameConfig.value.player_two_name = player_two_input.value,
        game_started.value = true
    }
    else {
        alert('Please enter your player names.')
    }
 
}
</script>

