<template>
    <div class="countdown">
        <div class="countdown-container">
            <img class="countdown-img" :src="img">
            <h1> Cuenta regresiva </h1>
            <h2> {{ timeLeft }}  min/seg </h2>
            <div class="countdown-buttons-container">
                <li v-for="(time, index) in times" :key="index">
                    <button @click.prevent="setTime(time.sec)">
                        {{ time.display }} 
                    </button>
                </li>
            </div>    
        </div>
    </div>
</template>

<script>

let intervalTimer;
        
// transforma 5 --> 05
// a futuro debería ser cambiado por 'computed'
function formatNumber(num) {
    return num < 10 ? `0${num}` : num;
}

export default {
name: 'app',
data() {
return {
  img:'http://blog.desafiolatam.com/wp-content/uploads/2015/03/desafio-latam-logonegro.png',
  selectedTime: 0,
  timeLeft: '00:00:00',
  endTime: '0',
  times: [
        {
            sec: 3,
            display: '3s'
        },
        {
            sec: 60,
            display: '1m'
        },
        {
            sec: 300,
            display: '5m'
        },
        {
            sec: 600,
            display: ' 10m'
        },
        {
            sec: 1800,
            display: '30m'
        }
    ]
    }
  },
    methods: {
        setTime(seconds) {
        clearInterval(intervalTimer);
        this.timer(seconds);
        },
        timer(seconds) {
        const now = Date.now();
        const end = now + seconds * 1000;
        this.displayTimeLeft(seconds);

        this.selectedTime = seconds;
        this.initialTime = seconds;
        this.countdown(end);
        },
        countdown(end) {
        this.initialTime = this.selectedTime;
        intervalTimer = setInterval(() => {
            const secondsLeft = Math.round((end - Date.now()) / 1000);

            if(secondsLeft === 0) {
            this.endTime = 0;
            }

            if(secondsLeft < 0) {
            clearInterval(intervalTimer);
            return;
            }
            this.displayTimeLeft(secondsLeft)
        }, 1000);
        },
        displayTimeLeft(secondsLeft) {
        const minutes = Math.floor((secondsLeft % 3600) / 60);
        const seconds = secondsLeft % 60;

        this.timeLeft = `${formatNumber(minutes)}:${formatNumber(seconds)}`;
        },
    }
};
</script>

<style scoped>
    .countdown {
        font-size: 18px;
        font-family: 'Calibri', sans-serif;
        display: flex;
        flex-flow: row nowrap;
        justify-content: space-around;
        width: 100%;
        height: 100%;
        margin: 10px;
    }

    .countdown-container {
        display: flex;
        flex-flow: column nowrap;
        border: 1px solid gray;
        padding: 20px;
        align-items: center;
    }
    li {
        list-style: none;
    }

    h2 {
        color: darkgray;
    }
    .countdown-img {
        width: 25%;
        height: 25%;
        object-fit: contain;
        display: flex;
        flex-flow: row nowrap;
        align-items:stretch;
    }

    .countdown-buttons-container{
        display: flex;
        flex-flow: row wrap;
        width: 90%;
        justify-content: space-around;
    }
    button {
        border: 0;
        background: none;
        box-shadow: none;
        border-radius: 0;
        background-color: lightgray;
    }
</style>