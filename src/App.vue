<template>
<div id="app">
    <div class="timer">
        <h1>Vue.js Pomodoro Timer</h1>

        <h1 class="time">{{minutes}}:{{seconds}}</h1>
        <div class="controls">
            <i v-on:click="adjustTimer('+')" class="fa fa-plus" aria-hidden="true"></i>

            <i v-if="running === false" v-on:click="toggleTimer()" class="fa fa-play" aria-hidden="true"></i>

            <i v-else v-on:click="toggleTimer()" class="fa fa-pause" aria-hidden="true"></i>

            <i v-on:click="adjustTimer('-')" class="fa fa-minus" aria-hidden="true"></i>
        </div>
        <div class="controls">

            <i v-on:click="resetTimer('-')" class="fa fa-refresh" aria-hidden="true"></i>

        </div>
    </div>

    <div class="credits">
        <i class="fa fa-github" aria-hidden="true"></i>
    </div>
</div>
</template>

<script>
export default {
    name: 'app',
    data() {
        return {
            now: Math.trunc((new Date()).getTime() / 1000),
            date: Math.trunc((new Date()).getTime() / 1000 + 1500), //our starting time
            timerId: null,
            timeRemaining: Math.trunc((new Date()).getTime() / 1000 + 1500),
            sessionLength: '1500',
            running: false


        }
    },
    computed: {
        seconds() {
            var seconds = (this.date - this.now) % 60;

            if (seconds.toString().length <= 1) {
                return seconds = "0" + seconds.toString(); // add leading zeros
            }
            return seconds.toString();
        },
        minutes() {
            return Math.trunc((this.date - this.now) / 60) % 60;
        }
        //http://fareez.info/blog/countdown-timer-using-vuejs/
    },
    methods: {
        adjustTimer: function(action) {

            if (this.running == false) {
                var diff = this.date - this.now;
                var seconds = (this.date - this.now) % 60;
                if (action == '-') {
                    if (diff > 60) {
                        this.date = this.date - 60; // subtract one minute
                        this.sessionLength = this.date - 60;
                    } else {
                        this.date = this.now + 60; // set to 60 seconds, timer cannot be less than 60 seconds
                        this.sessionLength = this.date + 60;
                    }
                } else { //action == '+'
                    this.date = this.date + 60; // add one minute
                    this.sessionLength = this.date + 60;
                }
            }

        },
        toggleTimer: function() {
            // @TODO add logic to account for time passed when the the timer is paused
            if (this.running === false) {

                this.running = true;

                this.now = Math.trunc((new Date()).getTime() / 1000);

                if (this.date - this.now < 1500) {
                    this.date = Math.trunc((new Date()).getTime() / 1000 + 1500);
                }

                this.timerId = window.setInterval(() => {
                    this.now = Math.trunc((new Date()).getTime() / 1000);
                    this.timeRemaining = Math.trunc((new Date()).getTime() / 1000);
                    console.log(this.timeRemaining)
                    if (this.now == this.date) {
                        this.stopTimer();
                        alert('Timer Done')
                    }
                }, 1000);

            } else {
                this.running = false;
                this.now = this.timeRemaining;
                return window.clearInterval(this.timerId)
            }


        },

        resetTimer: function() {
            this.running = false;
            this.now = Math.trunc((new Date()).getTime() / 1000);
            this.date = Math.trunc((new Date()).getTime() / 1000 + 1500);
            return window.clearInterval(this.timerId)

        }
    }
}
</script>

<style>
html {
    height: 100%;
}

body {
    height: 100%;
}

#app {
    height: 100%
}

.timer {
    height: 80%;
}

.credits {
    height: 20%;
}

#app {
    font-family: 'Open Sans', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: rgba(127, 140, 141, 1.0);
    margin-top: 0
}

h1,
h2 {
    font-weight: normal;
}

.time {
    font-size: 5rem;
    font-weight: 300;
    padding-bottom: 10px;
    margin-bottom: 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}

i {
    padding: 10px;
    margin: 0 5px;
    color: rgba(52, 73, 94, 1.0);
    cursor: pointer;
}

i:hover {
    color: rgba(52, 73, 94, 1.0);
}

.fa-play,
.fa-plus,
.fa-minus,
.fa-refresh {
    color: rgba(189, 195, 199, 1.0);
}
</style>
le>
