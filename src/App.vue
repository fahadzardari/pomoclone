<template >

  <div class="min-h-screen  transition duration-500" :class="
    tab == 1 ? 'bg-[#D95550]' : tab == 2 ? 'bg-[#4c9195]' : 'bg-[#457d9f]'
  ">

    <div class="grid md:grid-cols-4">
      <div></div>

      <div class="container col-span-2 ">
        <div class="flex flex-row justify-between py-4 text-sm md:text-lg text-white">
          <div>
            <h1 class="font-extrabold">PomoFocus</h1>
          </div>
          <div class="text-sm flex">
            <button class="bg-white bg-opacity-20 px-2 rounded-md"
              @click="settingsShow = !settingsShow">Settings</button>
          </div>
        </div>
        <div class="h-1 bg-gray-600 rounded-lg">
          <div :style="`width:`+progressBar+ `%;`" :class='`h-1   bg-gray-300`'>

          </div>
        </div>

        <div class="container flex justify-center items-center">
          <div class="bg-white bg-opacity-20 p-4 w-8/12 items-center rounded-md mt-6">

            <div class="flex flex-row items-center justify-center  text-white text-sm md:text-md lg:text-lg ">
              <div class="mx-2"
                :class="(tab === 1) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer  ` : `cursor-pointer`"
                @click="switchTab(1)">
                <a>Pomodoro</a>
              </div>
              <div class="mx-2"
                :class="(tab === 2) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer ` : ` cursor-pointer`"
                @click="switchTab(2)">
                <a>Short Break</a>
              </div>
              <div class="mx-2"
                :class="(tab === 3) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer ` : `cursor-pointer`"
                @click="switchTab(3)">
                <a>Long Break</a>
              </div>
            </div>
            <div class="text-center p-3 ">
              <span class="text-6xl proportional-nums md:text-9xl text-white ">
                {{ timeToShow }}
              </span>
            </div>

            <div class="buttons flex items-center justify-center font-bold text-xl ">

              <button class="py-3 px-10 bg-white border-4 border-b-black " v-if="!timerRunning"  @click="startTimer()">
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
              <button class="py-3 px-10 bg-white  border" v-else  @click="stopTimer()">
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
              <div class="timesettings py-5 ">
                <span class="font-bold">Time (minutes)</span>
                <div class="flex flex-row">
                  <div>
                    <label for="pomodoroTime" class="text-gray-400 ">Pomodoro</label> <br>
                    <input type="number" name="pomodoroTime" min="0" :value="pomodoroTime / 60"
                      @change="tabTimeChanged($event, 1)"
                      class="bg-gray-300 bg-opacity-30 w-7/12 font-light rounded-md px-2 py-2">
                  </div>
                  <div>
                    <label for="pomodoroTime" class="text-gray-400 ">Short Break</label> <br>
                    <input type="number" name="pomodoroTime" min="0" :value="shortBreakTime / 60"
                      @change="tabTimeChanged($event, 2)"
                      class="bg-gray-300 bg-opacity-30 w-7/12 font-light rounded-md px-2 py-2">
                  </div>
                  <div>
                    <label for="pomodoroTime" class="text-gray-400 ">Long Break</label> <br>
                    <input type="number" name="pomodoroTime" min="0" :value="longBreakTime / 60"
                      @change="tabTimeChanged($event, 3)"
                      class="bg-gray-300 bg-opacity-30 w-8/12 font-light rounded-md px-2 py-2">
                  </div>
                </div>
              </div>
              <div class="flex flex-row justify-between py-5 pr-7">
                <span class="text-lg font-bold">Auto Starts Break?</span>
                <label for="check1" class="relative rounded-full  cursor-pointer w-16 h-8"
                  :class="autoStartBreak ? 'bg-[#84c733]' : 'bg-[#cccccc]'">
                  <input type="checkbox" id="check1" v-model="autoStartBreak" class="sr-only peer">
                  <span class="bg-white w-2/5 h-4/5 absolute rounded-full left-1 top-[3px] peer-checked:left-9 "></span>

                </label>
              </div>
              <div class="flex flex-row justify-between py-5 pr-7">
                <span class="text-lg font-bold">Auto Starts Pomodoro?</span>
                <label for="check2" class="relative rounded-full  cursor-pointer w-16 h-8"
                  :class="autoStartPomodoro ? 'bg-[#84c733]' : 'bg-[#cccccc]'">
                  <input type="checkbox" id="check2" v-model="autoStartPomodoro" class="sr-only peer">
                  <span class="bg-white w-2/5 h-4/5 absolute rounded-full left-1 top-[3px] peer-checked:left-9 "></span>

                </label>
              </div>
              <div class="flex flex-row justify-between py-5 pr-7">
                
                    <label for="interval" class="text-lg font-bold ">Long Break Interval</label>
                    <input type="number" name="interval" min="1" :value="longBreakStartInterval"
                      @change="longBreakInterval($event)"
                      class="bg-gray-300 bg-opacity-30 w-2/12 font-light rounded-md px-2 py-2">
                
              </div>

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
      pomodoroTime: 3,
      shortBreakTime: 3,
      longBreakTime: 900,
      timeRemaining: 0,
      minutes: 0,
      seconds: 0,
      timeToShow: 0,
      timerRunning: false,
      interval: null,
      settingsShow: false,
      autoStartBreak: true,
      autoStartPomodoro: true,
      longBreakStartInterval: 4,
      totalTime:0,
      progressBar: 0,


    }
  },

  mounted() {
    this.pomodoroTime = localStorage.getItem("pomodoroTime") || 1500;
    this.resetTimeRemaining(this.pomodoroTime);

    this.shortBreakTime = localStorage.getItem("shortBreakTime") || 300;
    this.longBreakTime = localStorage.getItem("longBreakTime") || 900;
    this.autoStartBreak = localStorage.getitem("autoStartBreak") || true;
    this.autoStartPomodoro = localStorage.getItem("autoStartPomodoro") || true;
    this.longBreakStartInterval = localStorage.getItem("longBreakInterval") || 4;

  },

  methods: {
    startTimer() {
      this.timerRunning = true;
      this.timer();
    },
    timer() {
      this.interval = setInterval(() => {
        this.timeRemaining--;
        this.progressBar = (this.totalTime - this.timeRemaining ) /this.totalTime * 100;
        this.updateTime();
        if (this.timeRemaining <= 0) {

          this.stopTimer();
          if (this.tab == 1) {

            if (this.autoStartBreak) {
              this.tab = 2;
              this.resetTimeRemaining(this.shortBreakTime);
              this.startTimer();
            }
          } else
            if (this.tab == 2 || this.tab == 3) {
              if (this.autoStartPomodoro) {
                this.tab = 1;
                this.resetTimeRemaining(this.pomodoroTime);
                this.startTimer();
              }
            }
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
    switchTab(tabNumber) {
      if (this.timerRunning) {
        if (!confirm('The timer is still running, are you sure you want to switch?')) {
          exit;
        }
      }

      if (tabNumber == 1) {
        this.tab = 1;
        this.stopTimer();
        this.resetTimeRemaining(this.pomodoroTime);

      }
      if (tabNumber == 2) {
        this.tab = 2;
        this.stopTimer();
        this.resetTimeRemaining(this.shortBreakTime);

      }
      if (tabNumber == 3) {
        this.tab = 3;
        this.stopTimer();
        this.resetTimeRemaining(this.longBreakTime);

      }

    },
    resetTimeRemaining(time) {
      this.timeRemaining = time;
      this.totalTime = time;
      this.updateTime();
    },
    tabTimeChanged(event, tabNumber) {
      if (tabNumber == 1) {
        this.pomodoroTime = event.target.value * 60;
        localStorage.setItem("pomodoroTime", this.pomodoroTime);
      }
      if (tabNumber == 2) {
        this.shortBreakTime = event.target.value * 60;
        localStorage.setItem("shortBreakTime", this.shortBreakTime);

      }
      if (tabNumber == 3) {
        this.longBreakTime = event.target.value * 60;
        localStorage.setItem("longBreakTime", this.longBreakTime);
      }
      this.switchTab(this.tab);
    },
    longBreakInterval(event){
      this.longBreakStartInterval = event.target.value;
      localStorage.setItem("longBreakInterval" , this.longBreakStartInterval);
    },
    confirmChangeTab(tabNumber) {
      if (confirm('The timer is still running, are you sure you want to switch?')) {
        this.tab = tabNumber;
        this.stopTimer();
        switch (tabNumber) {
          case 1:
            this.resetTimeRemaining(this.pomodoroTime);
            break;
          case 2:
            this.resetTimeRemaining(this.shortBreakTime);
            break;
          case 3:
            this.resetTimeRemaining(this.longBreakTime);
            break;

        }
      }
    }


  },
  props: {

  },


}
</script>