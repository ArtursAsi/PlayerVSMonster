<template>
    <div class="container">
        <section class="players d-flex my-4">
            {{stopGame()}}

            <div class="player">
                <h4 class="player-name text-center">You</h4>
                <div class="health-bar">
                    <b-progress-bar class="health-remaining"  :style="{width:playerHealth + '%'}"></b-progress-bar>
                    <p class="health ">{{ playerHealth }}</p>
                </div>
            </div>

            <div class="player">
                <h4 class="player-name text-center">Monster</h4>
                <div class="health-bar">
                    <p class="health">{{ monsterHealth }}</p>
                    <b-progress-bar class="health-remaining " :style="{width: monsterHealth + '%'}"></b-progress-bar>
                </div>
            </div>
        </section>

        <b-card class="mb-2 text-center d-flex">
            <div v-show="gameIsRunning">
                <b-button @click="attack()" variant="danger" class="mr-2">Attack</b-button>
                <b-button @click="specialAttack()" variant="warning" class="mr-2">Special Attack</b-button>
                <b-button @click="heal()" variant="success" class="mr-2">Heal</b-button>
                <b-button @click="gameIsRunning = !gameIsRunning" variant="primary" class="mr-2">Give up
                </b-button>
            </div>

            <div v-show="!gameIsRunning">
                <b-button variant="success" @click="giveUp()">Start new game</b-button>
            </div>
        </b-card>

        <b-card class="mb-2 text-center d-flex">
            <div v-for="(turn,index) in turns" :key="index">
                <ul>
                    <li v-bind:style="index % 2 === 0 ? style='background-color:lightcoral; color:red' : style='background-color:lightblue; color:blue'">
                        {{turn}}
                    </li>
                </ul>
            </div>
        </b-card>
    </div>
</template>
<script>

    export default {
        data() {
            return {
                playerHealth: 100,
                monsterHealth: 100,
                turns: [],
                gameIsRunning: false
            };
        },
        methods: {
            attack() {
                const monster = Math.floor(Math.random() * 6) + 5;
                const player = Math.floor(Math.random() * 6) + 5;
                this.playerHealth -= monster;
                this.monsterHealth -= player;
                this.turns.unshift('Monster hits player for ' + monster);
                this.turns.unshift('Player hits monster for ' + player)

            },
            heal() {
                const monster = Math.floor(Math.random() * 6) + 5;
                if (this.playerHealth <= 95) {
                    this.playerHealth += 10;
                    this.playerHealth -= monster;
                    this.turns.unshift('Monster hits player for ' + monster);
                    this.turns.unshift('Player heals  10')

                }
            },
            specialAttack() {
                const monster = Math.floor(Math.random() * 6) + 5;
                const player = Math.floor(Math.random() * 5) + 10;
                this.playerHealth -= monster;
                this.monsterHealth -= player;
                this.turns.unshift('Monster hits player for ' + monster);
                this.turns.unshift('Player hits monster hard for ' + player)
            },
            stopGame() {
                if (this.monsterHealth <= 0) {
                    this.fullHp();
                    this.turns = [];
                    this.giveUp();
                    alert('You won');

                }
                if (this.playerHealth <= 0) {
                    this.fullHp();
                    this.turns = [];
                    this.giveUp();
                    alert('You lost');

                }
            },
            fullHp() {
                this.playerHealth = 100;
                this.monsterHealth = 100;
            },
            giveUp() {
                this.gameIsRunning = !this.gameIsRunning;
                this.turns = [];
                this.fullHp();


            },

        },


    };
</script>
<style scoped>

    .container {
        max-width: 900px;
        width: 90%;
    }

    .player {
        width: 50%;
    }

    .health-bar {
        position: relative;
        display: flex;
        margin: auto;
        width: 90%;
        height: 40px;
        background-color: grey;


    }

    .health {
        z-index: 100;
        text-align: center;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }


    .health-remaining {

        background-color: green;
        height: 40px;
        position: absolute;



    }

    ul {
        list-style-type: none;
        margin: 2px;
    }

</style>