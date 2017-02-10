<template>
<div id="app">

    <h1>Vue.js Pomodoro Timer</h1>

    <h1 class="time">{{minutes}}:{{seconds}}</h1>
    <div class="controls">
        <i v-on:click="adjustTimer('+')" class="fa fa-plus" aria-hidden="true"></i>

        <i v-if="running === false" v-on:click="startTimer()" class="fa fa-play" aria-hidden="true"></i>

        <i v-else v-on:click="stopTimer()" class="fa fa-pause" aria-hidden="true"></i>

        <i v-on:click="adjustTimer('-')" class="fa fa-minus" aria-hidden="true"></i>
    </div>
    <div class="controls">

        <i v-on:click="resetTimer('-')" class="fa fa-refresh" aria-hidden="true"></i>

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
            timer: '',
            running: false

            //http://fareez.info/blog/countdown-timer-using-vuejs/
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
    },
    methods: {
        adjustTimer: function(action) {

            if (this.running == false) {
                var diff = this.date - this.now;
                var seconds = (this.date - this.now) % 60;

                if (action == '-') {
                    if (diff > 60) {
                        this.date = this.date - 60; // subtract one minute
                    } else {
                        this.date = this.now; // set to zero if less than 60 seconds
                    }
                } else { //action == '+'
                    this.date = this.date + 60; // add one minute
                }
            }

        },
        startTimer: function() {

            this.running = true;

            this.timer = window.setInterval(() => {
                this.now = Math.trunc((new Date()).getTime() / 1000);
                if (this.now == this.date) {
                    this.stopTimer();
                    alert('Timer Done')
                }
            }, 1000);

        },

        stopTimer: function() {
            this.running = false;
            return window.clearInterval(this.timer)
        },
        resetTimer: function() {
            this.running = false;
            this.stopTimer();
            return this.date = this.now + 1500;
        }
    }
}
</script>

<style>
#app {
    font-family: 'Open Sans', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: rgba(127, 140, 141, 1.0);
    margin-top: 60px;
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
