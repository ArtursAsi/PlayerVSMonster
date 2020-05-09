<template>
    <div class="container">
        <section class="players d-flex my-4">

            <div class="player">
                <h4 class="player-name text-center">You</h4>
                <div class="health-bar">
                    <b-progress-bar class="health-remaining" :style="{width:playerHealth + '%'}"></b-progress-bar>
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
                <b-button @click="giveUp()" variant="primary" class="mr-2">Give up
                </b-button>
            </div>

            <div v-show="!gameIsRunning">
                <b-button variant="success" @click="startGame()">Start new game</b-button>
            </div>
        </b-card>

        <b-card class="mb-2 text-center d-flex">
            <div v-for="(turn,index) in turns" :key="index">
                <ul>
                    <li :style="index % 2 !== 0 ? style='background-color:lightcoral; color:red' : style='background-color:lightblue; color:blue'">
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
            playerAttack() {
                this.stopGame();
                const player = Math.floor(Math.random() * 6) + 5;
                this.monsterHealth -= player;
                this.turns.unshift('Player hits monster for ' + player);


            },
            startGame() {
                this.playerHealth = 100
                this.monsterHealth = 100
                this.turns = []
                this.gameIsRunning = true;
            },
            monsterAttack() {
                this.stopGame();
                const monster = Math.floor(Math.random() * 6) + 5;
                this.playerHealth -= monster;
                this.turns.unshift('Monster hits player for ' + monster);


            },
            heal() {
                if (this.playerHealth <= 95) {
                    this.monsterAttack();
                    this.playerHealth += 10;
                    this.turns.unshift('Player heals  10')

                }
            },
            attack() {
                this.monsterAttack();
                this.playerAttack()


            },
            specialAttack() {
                this.stopGame();
                this.monsterAttack();
                const player = Math.floor(Math.random() * 5) + 10;
                this.monsterHealth -= player;
                this.turns.unshift('Player hits monster hard for ' + player)
            },
            stopGame() {
                if (this.monsterHealth <= 0) {
                    if (confirm('You won! New Game?')) {
                        this.startGame()
                    } else {
                        this.gameIsRunning = false;
                    }
                    return true;

                } else if (this.playerHealth <= 0) {
                    if (confirm('You lost! New Game?')) {
                        this.startGame()
                    } else {
                        this.gameIsRunning = false;
                    }
                    return true;

                }
                return false;
            },

            giveUp() {
                this.gameIsRunning = !this.gameIsRunning;
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