<template >
  <div class="min-h-screen bg-[#D95550] transition-all ">

    <div class="grid grid-cols-3">
      <div></div>

      <div>
        <h1 class="text-2xl font-extrabold text-center text-white"> POMOFOCUS</h1>
        <div class="container bg-white bg-opacity-20 p-4 w-full rounded-md">

          <div class="flex flex-row justify-between text-white text-xl  ">
            <div
              :class="(tab === 1) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : `cursor-pointer`"
              @click="tab = 1">
              <a>Pomodoro</a>
            </div>
            <div
              :class="(tab === 2) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : ` cursor-pointer`"
              @click="tab = 2">
              <a>Short Break</a>
            </div>
            <div
              :class="(tab === 3) ? ` bg-black bg-opacity-20 px-2 py-1 rounded-md font-bold cursor-pointer text-lg` : `cursor-pointer`"
              @click="tab = 3">
              <a>Long Break</a>
            </div>
          </div>

          <div class="text-center p-3 ">
            <span class="text-7xl text-white font-bold">
              {{ timeToShow }}
            </span>
          </div>

          <div class="buttons flex items-center justify-center ">

            <button class="" @click="timerRunning()">START</button>

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
      timeRemaining: 1512,
      minutes: 0,
      seconds: 0,
      timeToShow: 0,
      remainder: 0 ,

    }
  },

  mounted() {
    this.setTime();
    this.timeToShow = this.minutes + ":" + this.seconds;
    document.title = this.timeToShow + " - Time remaining to complete cycle";
  },

  methods: {
    timerRunning() {
      setInterval(() => {
        this.setTime();
        this.timeRemaining--;
      }, 1000);

    },
        setTime() {
          this.seconds = this.timeRemaining % 60 ;
          this.minutes = Math.floor(this.timeRemaining / 60);
     
        },

  },
  props: {

  },


}
</script>