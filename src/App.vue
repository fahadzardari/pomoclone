<template >
  <div class="min-h-screen  transition duration-500" :class="
    tab == 1 ? 'bg-[#D95550]' : tab == 2 ? 'bg-[#4c9195]' : 'bg-[#457d9f]'
  ">

    <div class="grid md:grid-cols-4">
      <div></div>

      <div class="container col-span-2 divide-y-[1px] divide-black divide-opacity-20">
        <div class="flex flex-row justify-between py-4 text-sm md:text-lg text-white">
          <div>
            <h1 class="font-extrabold">PomoFocus</h1>
          </div>
          <div class="text-sm flex">
            <button class="bg-white bg-opacity-20 px-2 rounded-md"
              @click="settingsShow = !settingsShow">Settings</button>
          </div>
        </div>
        <div class="container flex justify-center items-center">
          <div class="bg-white bg-opacity-20 p-4 w-8/12 items-center rounded-md mt-6">

            <div class="flex flex-row items-center justify-center  text-white text-sm md:text-md lg:text-lg ">
              <div class="mx-2"
                :class="(tab === 1) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer  ` : `cursor-pointer`"
                @click="switchTab('pomo')">
                <a>Pomodoro</a>
              </div>
              <div class="mx-2"
                :class="(tab === 2) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer ` : ` cursor-pointer`"
                @click="switchTab('short')">
                <a>Short Break</a>
              </div>
              <div class="mx-2"
                :class="(tab === 3) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer ` : `cursor-pointer`"
                @click="switchTab('long')">
                <a>Long Break</a>
              </div>
            </div>
            <div class="text-center p-3 ">
              <span class="text-6xl proportional-nums md:text-9xl text-white ">
                {{ timeToShow }}
              </span>
            </div>

            <div class="buttons flex items-center justify-center font-bold text-xl ">

              <button class="py-4 px-6 bg-white border-4 border-b-black " v-show="!timerRunning" @click="startTimer()">
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
              <button class="py-4 px-6 bg-white  border" v-show="timerRunning" @click="stopTimer()">
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
          <div v-if="settingsShow"
            class="bg-[#00000066] flex justify-center lg:px-8 py-20 p-2 fixed top-0 w-screen min-h-screen">
            <div
              class="settings bg-white w-9/12 md:w-4/12 rounded-md divide-y-[1px] divide-black divide-opacity-20 p-3">
              <div class="flex flex-row justify-between items-center mb-2">
                <span class="font-medium text-gray-400 text-opacity-50 capitalize">TIMER SETTINGS</span>
                <div class="cursor-pointer" @click="settingsShow = !settingsShow">
                  <img src="./assets/remove-black-sm.png" alt="close button" class="w-3 h-3 ">
                </div>
              </div>
              <div>sdf</div>
              <div>dsf</div>
              <div>sdf</div>

            </div>
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
      pomo: 1500,
      shortBreak: 300,
      longBreak: 900,
      timeRemaining: 0,
      minutes: 0,
      seconds: 0,
      timeToShow: 0,
      remainder: 0,
      timerRunning: false,
      interval: null,
      settingsShow: false

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
    timer() {
      this.interval = setInterval(() => {
        this.timeRemaining--;
        this.updateTime();
        if (this.timeRemaining <= 0) {

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
    switchTab(tabName) {
      if (tabName == 'pomo') {
        this.tab = 1;
        if (this.timerRunning == true) {
          //confirm('Timer is running if you want to continue press ok') ;
        }
        this.stopTimer();
        this.resetTime(this.pomo);
      }
      if (tabName == 'short') {
        this.tab = 2;
        this.stopTimer();
        this.resetTime(this.shortBreak);

      }
      if (tabName == 'long') {
        this.tab = 3;
        this.stopTimer();
        this.resetTime(this.longBreak);

      }
    },
    resetTime(time) {
      this.timeRemaining = time;
      this.updateTime();
    }

  },
  props: {

  },


}
</script>