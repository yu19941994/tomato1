<template>
  <div class="home">
    <div class="wrap" :class="{'open': isOpen}">
      <div class="container">
        <h1 class="font-h2">My First Task</h1>
        <div class="count">
          <div class="clock">
            <svg height="300" weight="300">
              <circle
                class="bg"
                cx="150"
                cy="150"
                r="125"
                stroke="#ACACAC"
                stroke-width="50"
                fill="#eaeaea"
              />
              <circle
                class="loading"
                cx="150"
                cy="150"
                r="125"
                stroke="#ea5548"
                stroke-width="50"
                fill="#eaeaea"
                :stroke-dasharray="circumference"
                :stroke-dashoffset="progress"
              />
            </svg>
            <p class="font font-h1"><span>{{ minutes }}</span>:<span>{{seconds}}</span></p>
          </div>
        </div>
        <div class="func-btn">
          <div class="circlebtn circlebtn-white" @click="startClock">
            <i class="fas fa-play"></i>
          </div>
          <div class="circlebtn circlebtn-white" @click="stopTimer">
            <i class="fas fa-pause"></i>
          </div>
          <div class="circlebtn circlebtn-white" @click="resetTimer">
            <i class="fas fa-undo-alt"></i>
          </div>
        </div>
        <p class="font font-h5 subtitle">TASK COMPLETE</p>
      </div>
      <!--Sidebar-->
      <div class="sidebarbox">
        <div class="sidebar">
          <div class="tasklist">
            <!--list-->
            <div class="list" v-if="menu === 'list'">
              <p class="font font-h3">TASK LISTS</p>
              <hr>
              <div class="badge badge-primary">DONE</div>
              <ul>
                <li>
                  <div>
                    <i class="far fa-check-circle"></i> 
                    <p class="font-h4">My First Task</p>
                  </div>
                  <i class="fas fa-ellipsis-h"></i>
                </li>
              </ul>
            </div>
            <!--graph-->
            <div class="graph" v-if="menu === 'graph'">
              <p class="font font-h3">ANALYTICS REPORT</p>
              <hr>
              <p class="font font-h5 totaltitle">TOMATO OF THIS WEEK</p>
              <div class="total">
                <div class="tomatos">
                  <p class="font font-text">8</p>
                </div>
                <div class="weekly">
                  <p class="font font-text">20</p>
                </div>
              </div>
              <p class="font font-h5 totaltitle">CHART</p>
              <div class="chart">
              </div>
              <div class="graphBtn">
                <div class="btn-outline btn-outline-oprimary">PREV</div>
                <div class="btn-outline btn-outline-oprimary">NEXT</div>
              </div>
            </div>
            <!--music-->
            <div class="music" v-if="menu === 'music'">
              <p class="font font-h3">RING TONG</p>
              <hr>
              <ul>
                <li>
                  <div>
                    <i class="far fa-check-circle"></i> 
                    <p class="font-h4">Ring tone1</p>
                  </div>
                  <i class="far fa-play-circle"></i>
                </li>
              </ul>
            </div>
          </div>
          <div class="toolbar">
              <ul>
                <li><i href="#" class="fas fa-plus-circle"></i></li>
                <li><i href="#" class="fas fa-bars" @click="menu = 'list'"></i></li>
                <li><i href="#" class="fas fa-chart-bar" @click="menu = 'graph'"></i></li>
                <li><i href="#" class="fas fa-music"  @click="menu = 'music'"></i></li>
              </ul>
          </div>
        </div>
      </div>
      <!--toggleButton-->
      <div class="tomatoIcon" @click="openSidebar">
        <div>
          <img src="../assets/stylesheets/image/tomato.png" alt="">
          <i class="fas fa-arrow-right" v-if="!isOpen"></i>
          <i class="fas fa-arrow-left" v-if="isOpen"></i>
        </div> 
      </div>
      
    </div>
  </div>
</template>

<script>
// import $ from 'jquery'

export default {
  name: 'Home',
  data () {
    return{
      isOpen: false,
      isStart: false,
      targetTime: 25*60, //換成毫秒
      minutes:'',
      seconds:'',
      now:0,
      percentage: 100,
      timer: '',
      count: '',
      menu: ''
    }
  },
  methods: {
    openSidebar () {
      this.isOpen = !this.isOpen
    },
  
    countTime () {
      if(this.targetTime>0){
        let seconds_left = (this.targetTime--)
        this.minutes = this.pad(parseInt(seconds_left / 60))
        this.seconds = this.pad(parseInt(seconds_left % 60))
      }
      
    },
    pad (n) {
      //如果小於10，給它'0x'
      return (n < 10 ? '0' : '')+n
    },
    
    startClock () {
      this.isStart =! this.isStart
      if(this.isStart == true){
        this.count = setInterval(() => {this.countTime()}, 1000)
      }
      this.rotate()
    },
    stopTimer () {
      clearInterval(this.count)
      clearInterval(this.timer)
      this.timer = null
      this.count = null
    },
    resetTimer () {
      this.targetTime = 25*60
      this.percentage = 100
    },
    rotate () {
      this.timer = setInterval(() => {
        this.percentage -=1/2500
        if(this.percentage <= 0) {
          clearInterval(this.timer)
          this.percentage = 100
        }
      }, this.seconds)
      
    }

  },
  components: {},
  computed: {
    circumference () {
      return 125*2*Math.PI
    },
    progress () {
      return this.circumference - this.circumference*this.percentage/100
    }
  },
  created () {
    this.countTime()
  }
}
</script>
