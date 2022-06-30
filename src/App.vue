<template >
  <div class="min-h-screen  transition duration-500" :class="
    tab == 1 ? 'bg-[#D95550]' : tab == 2 ? 'bg-[#4c9195]' : 'bg-[#457d9f]'
  ">

    <div class="grid grid-cols-3">
      <div></div>

      <div>
        <h1 class="text-2xl font-extrabold text-center text-white"> POMOFOCUS</h1>
        <div class="container bg-white bg-opacity-20 p-4 w-full rounded-md">

          <div class="flex flex-row justify-between text-white text-xl  ">
            <div
              :class="(tab === 1) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : `cursor-pointer`"
              @click="switchTab('pomo')">
              <a>Pomodoro</a>
            </div>
            <div
              :class="(tab === 2) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : ` cursor-pointer`"
              @click="switchTab('short')">
              <a>Short Break</a>
            </div>
            <div
              :class="(tab === 3) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : `cursor-pointer`"
              @click="switchTab('long')">
              <a>Long Break</a>
            </div>
          </div>

          <div class="text-center p-3 ">
            <span class="text-7xl text-white font-bold">
              {{ timeToShow }}
            </span>
          </div>

          <div class="buttons flex items-center justify-center font-bold text-xl ">

            <button class="py-4 px-6 bg-white " v-show="!timerRunning" @click="startTimer()">
              <span :class="
                tab == 1
                  ? 'text-[#D95550]'
                  : tab == 2
                    ? 'text-[#4c9195]'
                    : 'text-[#457d9f]'
              ">
                START
              </span>
            </button>
            <button class="py-4 px-6 bg-white " v-show="timerRunning" @click="stopTimer()">
              <span :class="
                tab == 1
                  ? 'text-[#D95550]'
                  : tab == 2
                    ? 'text-[#4c9195]'
                    : 'text-[#457d9f]'
              ">
                STOP
              </span>
            </button>


          </div>







        </div>
      </div>



      <div></div>
    </div>
  </div>

</template>

<script>

export default {
  data() {
    return {
      tab: 1,
      pomo:1500,
      shortBreak:300,
      longBreak:900,
      timeRemaining: 0,
      minutes: 0,
      seconds: 0,
      timeToShow: 0,
      remainder: 0,
      timerRunning: false,
      interval: null,

    }
  },

  mounted() {
    this.resetTime(this.pomo);
  },

  methods: {
    startTimer() {
      
      this.timerRunning = true;
      this.timer();      
    },
    timer(){
      this.interval = setInterval(() => {
        this.timeRemaining--;
        this.updateTime();
        if(this.timeRemaining <= 0){
          
          this.stopTimer();
        }
        
      }, 1000);

    },
    stopTimer() {
      this.timerRunning = false;
      clearInterval(this.interval);

    },
    updateTime() {
      this.seconds = this.timeRemaining % 60;
      this.minutes = Math.floor(this.timeRemaining / 60);
      if (this.seconds < 10) {
        this.seconds = "0" + this.seconds;
      }
      if (this.minutes < 10) {
        this.minutes = "0" + this.minutes;

      }
      this.timeToShow = this.minutes + ":" + this.seconds;
      document.title = this.timeToShow + " - Time remaining to complete cycle";


    },
    switchTab(tabName){
        if(tabName == 'pomo'){
          this.tab = 1;
          if(this.timerRunning == true){
            //confirm('Timer is running if you want to continue press ok') ;
          }
          this.stopTimer();
          this.resetTime(this.pomo);
        }
        if(tabName == 'short'){
          this.tab = 2;
          this.stopTimer();
          this.resetTime(this.shortBreak);

        }
        if(tabName == 'long'){
          this.tab = 3;
          this.stopTimer();
          this.resetTime(this.longBreak);

        }
    },
    resetTime(time){
      this.timeRemaining = time;
      this.updateTime();
    }

  },
  props: {

  },


}
</script>