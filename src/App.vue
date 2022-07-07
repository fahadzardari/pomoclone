<template >

  <div class="min-h-screen  transition duration-500" :class="
    tab == 1 ? 'bg-[#D95550]' : tab == 2 ? 'bg-[#4c9195]' : 'bg-[#457d9f]'
  ">

    <div class="grid md:grid-cols-4">
      <div></div>

      <div class="container col-span-2 ">
        <div class="flex flex-row justify-between py-4 text-sm md:text-lg text-white mx-4">
          <div>
            <h1 class="font-extrabold">PomoFocus</h1>
          </div>
          <div class="text-sm flex">
            <button class="bg-white bg-opacity-20 px-2 rounded-md"
              @click="settingsShow = !settingsShow">Settings</button>
          </div>
        </div>
        <div class="h-0.5 bg-gray-600 rounded-lg">
          <div :style="`width:` + progressBar + `%;`" :class='`h-0.5   bg-gray-300`'>

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
              <span class="text-white text-[75px] lg:text-[120px]  text-center font-bold ">
                {{ timeToShow }}
              </span>
            </div>

            <div class="buttons flex items-center justify-center font-bold text-xl ">

              <button class="py-3 px-10 bg-white border-4 border-b-black " v-if="!timerRunning" @click="startTimer()">
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
              <button class="py-3 px-10 bg-white  border" v-else @click="stopTimer()">
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
              class="settings bg-white w-9/12 md:w-4/12 rounded-md divide-y-[1px] divide-black divide-opacity-20 p-3 overflow-hidden  scroll-m-1">
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
                <label for="check1">
                  <input type="checkbox" id="check1" v-model="autoStartBreak" @change="autoStartBreakStore()"
                    class="sr-only">
                  <div class="relative rounded-full  cursor-pointer w-16 h-8"
                    :class="autoStartBreak ? 'bg-[#84c733]' : 'bg-[#cccccc]'">
                    <span class="bg-white w-2/5 h-4/5 absolute rounded-full top-[3px]  "
                      :class="autoStartBreak ? `left-9` : ` `"></span>
                  </div>

                </label>
              </div>
              <div class="flex flex-row justify-between py-5 pr-7">
                <span class="text-lg font-bold">Auto Starts Pomodoro?</span>
                <label for="check2" class="">
                  <input type="checkbox" id="check2" v-model="autoStartPomodoro" class="sr-only ">
                  <div class="relative rounded-full  cursor-pointer w-16 h-8"
                    :class="autoStartPomodoro ? 'bg-[#84c733]' : 'bg-[#cccccc]'">

                    <span class="bg-white w-2/5 h-4/5 absolute rounded-full top-[3px] "
                      :class="autoStartPomodoro ? 'left-9' : 'left-1'"></span>
                  </div>

                </label>
              </div>
              <div class="flex flex-row justify-between py-5 pr-7">

                <label for="interval" class="text-lg font-bold ">Long Break Interval</label>
                <input type="number" name="interval" min="1" :value="longBreakStartInterval"
                  @change="longBreakInterval($event)"
                  class="bg-gray-300 bg-opacity-30 w-2/12 font-light rounded-md px-2 py-2">

              </div>

              <div class="flex flex-row justify-between py-5 pr-7">
                <label for="tickingSound" class="text-lg font-bold ">Alarm Sound</label>
                <select name="tickingSound" v-model="tickingSound" class="px-3 py-2 rounded-md " @change="tickingSoundStore()">
                  <option  value="0">None</option>
                  <option  value="1">Ticking Slow</option>
                  <option  value="2">Ticking Fast</option>
                    
                </select>
              </div>
       
          </div>






        </div>
      </div>



      <div></div>
    </div>
  </div>
  </div>
  <div >
    <div class="md:w-[620px] w-full p-2 mx-auto">
      <div class="w-full pt-12">
        <h1 class="md:text-[34px] text-[24px] leading-10 text-[#541c1f] font-bold leading-0">
          An online Pomodoro Timer to boost your productivity
        </h1>
        <!-- <image src="/images/heroimage2.png" class="heroimage__img"></image> -->
      </div>

      <div class="mt-14">
        <h2 class="heading text-[24px] leading-10 text-[#541c1f] font-bold leading-0">
          What is Pomofocus?
        </h2>
        <p class="py-4 text-[#785e60] text-[18px]">
          Pomofocus is a customizable pomodoro timer that works on desktop &
          mobile browser. The aim of this app is to help you focus on any task
          you are working on, such as study, writing, or coding. This app is
          inspired by
          <a href="https://francescocirillo.com/pages/pomodoro-technique" target="_blank" rel="noopener">Pomodoro
            Technique</a>
          which is a time management method developed by Francesco Cirillo.
        </p>
      </div>

      <div class="mt-8">
        <h2 class="heading text-[24px] leading-10 text-[#541c1f] font-bold leading-0">
          What is Pomodoro Technique?
        </h2>
        <p class="py-4 text-[#785e60] text-[18px]">
          The Pomodoro Technique is created by Francesco Cirillo for a more
          productive way to work and study. The technique uses a timer to break
          down work into intervals, traditionally 25 minutes in length,
          separated by short breaks. Each interval is known as a pomodoro, from
          the Italian word for 'tomato', after the tomato-shaped kitchen timer
          that Cirillo used as a university student. -
          <a href="https://en.wikipedia.org/wiki/Pomodoro_Technique" target="_blank" rel="noopener">Wikipedia</a>
        </p>
      </div>

      <div class="mt-8">
        <h2 class="heading text-[24px] leading-10 text-[#541c1f] font-bold leading-0">
          How to use the Pomodoro Timer?
        </h2>
        <ul class="py-2 pl-1">
          <li class="text-[#785e60] text-[16px]">
            <span class="text-[#785e60] px-2">1.</span>
            <strong class="text-[#785e60] font-bold">Add tasks</strong> to work
            on today
          </li>
          <li class="text-[#785e60] text-[16px] mt-3">
            <span class="text-[#785e60] px-2">2.</span>
            <strong class="text-[#785e60] font-bold">Set estimate pomodoros</strong>
            (1 = 25min of work) for each tasks
          </li>
          <li class="text-[#785e60] text-[16px] mt-3">
            <span class="text-[#785e60] px-2">3.</span>
            <strong class="text-[#785e60] font-bold">Select a task</strong> to
            work on
          </li>
          <li class="text-[#785e60] text-[16px] mt-3">
            <span class="text-[#785e60] px-2">4.</span>
            <strong class="text-[#785e60] font-bold">Start timer</strong> and
            focus on the task for 25 minutes
          </li>
          <li class="text-[#785e60] text-[16px] mt-3">
            <span class="text-[#785e60] px-2">5.</span>
            <strong class="text-[#785e60] font-bold">Take a break</strong> for 5
            minutes when the alarm ring
          </li>
          <li class="text-[#785e60] text-[16px] mt-3">
            <span class="text-[#785e60] px-2">6.</span>
            <strong class="text-[#785e60] font-bold">Iterate</strong> 3-5 until
            you finish the tasks
          </li>
        </ul>
      </div>

      <div class="mt-6">
        <h2 class="heading md:text-[24px] text-[20px] leading-10 text-[#541c1f] font-bold leading-0">
          Features
        </h2>
        <ul class="list-disc py-4 pl-8">
          <li class="text-[#785e60] text-[18px] mt-3">
            <strong class="text-[#785e60] font-bold">Responsive design</strong>
            that works with desktop and mobile
          </li>
          <li class="text-[#785e60] text-[18px] mt-3">
            <strong class="text-[#785e60] font-bold">Color transition</strong>
            to switch moods between work time and rest time
          </li>
          <li class="text-[#785e60] text-[18px] mt-3">
            <strong class="text-[#785e60] font-bold">Audio notification</strong>
            at the end of a timer period
          </li>
          <li class="text-[#785e60] text-[18px] mt-3">
            <strong class="text-[#785e60] font-bold">Customizable timer</strong>
            intervals to suit your preference
          </li>
        </ul>
      </div>
    </div>

    <hr />

    <div class="lg:w-4/12 md:w-10/12 mt-4 w-full text-center p-2 mx-auto">
      <a class="px-2 text-[14px] font-bold text-[#785e60] cursor-pointer" href="/">HOME</a>
      <a class="px-2 text-[14px] font-bold text-[#785e60] cursor-pointer" href="/privacy">PRIVACY</a>
      <a class="px-2 text-[14px] font-bold text-[#785e60] cursor-pointer" href="mailto:pomofocus@gmail.com">CONTACT</a>
      <a class="px-2 text-[14px] font-bold text-[#785e60] cursor-pointer" href="/app">SIMPLE PAGE</a>
    </div>

    <div class="flex justify-center items-center gap-4 mt-4 mb-6">
      <img src="./assets/facebook.png" class="w-10 h-10 bg-gray-400 p-1 rounded-full" alt="" />

      <img src="./assets/twitter.png" class="w-10 h-10 bg-gray-400 p-1 rounded-full" alt="" />

      <img src="./assets/stripe-climate-badge.png" class="w-10 h-10 bg-gray-400 p-1 rounded-full" alt="" />
    </div>

    <div class="text-center my-4 text-[#785e60]">
      Made by <span class="text-[#d95550] font-bold">Sagheer Hussain</span>
    </div>

    <div class="text-center my-4 text-[#785e60] text-xs">
      ©Pomofocus 2022. All Rights Reserved.
    </div>
  </div>
</template>

<script>
import ticking_slow from "./assets/audio/ticking-slow.mp3";
import ticking_fast from "./assets/audio/ticking-fast.mp3";
import alarm_bell from "./assets/audio/alarm-bell.mp3";
import alarm_bird from "./assets/audio/alarm-bird.mp3";
import alarm_digital from "./assets/audio/alarm-digital.mp3";
import alarm_kitchen from "./assets/audio/alarm-kitchen.mp3";
import alarm_wood from "./assets/audio/alarm-wood.mp3";

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
      autoStartBreak: false,
      autoStartPomodoro: false,
      longBreakStartInterval: 4,
      counter: 0,
      totalTime: 0,
      progressBar: 0,
      currentAudio: null,
      tickingSound: null,
      alarmSound: alarm_bell


    }
  },

  mounted() {

    this.pomodoroTime = localStorage.getItem("pomodoroTime") || 1500;
    this.shortBreakTime = localStorage.getItem("shortBreakTime") || 300;
    //this.pomodoroTime = 3;
    //this.shortBreakTime = 3;
    this.autoStartBreak = localStorage.getItem("autoStartBreak") || false;
    this.resetTimeRemaining(this.pomodoroTime);
    this.longBreakStartInterval = localStorage.getItem("longBreakInterval") || 4;
    this.longBreakTime = localStorage.getItem("longBreakTime") || 900;
    this.tickingSound = localStorage.getItem("tickingSound") || 1;
    //this.autoStartPomodoro = localStorage.getitem("autoStartBreak") || false;
    this.currentAudio = new Audio();
  },

  methods: {
    startTimer() {
      this.timerRunning = true;
      this.timer();
    },
    timer() {
      this.tickingSoundPlay();
      this.interval = setInterval(() => {
        this.timeRemaining--;
        this.progressBar = (this.totalTime - this.timeRemaining) / this.totalTime * 100;
        this.updateTime();
        if (this.timeRemaining <= 0) {

          this.stopTimer();
          if (this.tab == 1) {

            if (this.autoStartBreak) {
              // if(this.counter == this.longBreakStartInterval){
              //     alert('inside long break interval');
              //     this.counter =0;
              //     this.tab=3;
              //     this.resetTimeRemaining(this.longBreakTime);

              //     this.startTimer();

              // }
              this.switchTab(2);
              this.startTimer();
              this.shortBreakCounter();
            }
          } else
            if (this.tab == 2 || this.tab == 3) {
              if (this.autoStartPomodoro) {
                this.switchTab(1);
                this.startTimer();
              }
            }
        }

      }, 1000);

    },
    stopTimer() {
      this.timerRunning = false;
      this.currentAudio.pause();
      clearInterval(this.interval);
      //if (this.timerRunning) {
      //} 

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
      this.stopTimer();
      if (tabNumber == 1) {
        this.tab = 1;
        this.resetTimeRemaining(this.pomodoroTime);

      } else
        if (tabNumber == 2) {
          this.tab = 2;
          this.resetTimeRemaining(this.shortBreakTime);

        } else
          if (tabNumber == 3) {
            this.tab = 3;
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
    longBreakInterval(event) {
      this.longBreakStartInterval = event.target.value;
      localStorage.setItem("longBreakInterval", this.longBreakStartInterval);
    },
    shortBreakCounter() {
      if (this.tab === 2) {
        //alert(this.counter);
        this.counter++;
      }
    },
    autoStartBreakStore() {
      localStorage.setItem("autoStartBreak", this.autoStartBreak);
      alert(localStorage.getItem("autoStartBreak"));
    },
    autoStartPomodoroStore() {
      localStorage.setItem("autoStartPomodoro", this.autoStartPomodoro);
    },
    tickingSoundStore(){
        localStorage.setItem("tickingSound" , this.tickingSound);
    },
    tickingSoundPlay() {
      if (this.tickingSound != 0) {
        
        this.currentAudio.src = (this.tickingSound == 1) ? ticking_slow : ticking_fast ;
        this.currentAudio.load();
        this.currentAudio.loop = true;
        this.currentAudio.play();
      }
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