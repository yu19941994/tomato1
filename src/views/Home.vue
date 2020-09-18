<template>
  <div class="home">
    <div class="wrap" :class="{'open': isOpen}">
      <!--Sidebar-->
      <div class="sidebarbox">
        <div class="sidebar">
          <div class="tasklist">
             <!--addNew-->
            <div class="addNew" v-if="menu === 'addNew'">
              <p class="font font-h3">ADD NEW TASK</p>
              <hr>
              <p class="font font-h5 totaltitle">TASK TITLE</p>
              <form>
                <label for=""></label>
                <input type="text" placeholder="My Second Task" v-model="newTodoTitle" @keyup.enter="addTodo">
              </form>
              <p class="font font-h5 totaltitle">ESTIMATE TOMATO</p>
              <div class="tomatoCount">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
              </div>
              <div class="btn btn-primary" @click="addTodo">ADD TASK</div>
            </div>
            <!--list-->
            <div class="list" v-if="menu === 'list'">
              <p class="font font-h3">TASK LISTS</p>
              <hr>
              <div class="badge-group">
                <div class="badge badge-secondary-light"
                :class="{'active':visibility == 'todo'}"
                @click.prevent="visibility = 'todo'">TODO</div>
                <div class="badge badge-secondary-light"
                :class="{'active':visibility == 'done'}"
                @click.prevent="visibility = 'done'">DONE</div>
              </div>
              <ul>
                <li v-for="item in filterTodos" :key="item.id" @click="chosenTomato">
                  <div class="listTitle">
                    <div>
                      <i class="far fa-check-circle"></i> 
                      <p class="font font-h4">{{item.title}}</p>
                    </div>
                    <i class="fas fa-ellipsis-h" @click="editTodos(item)"></i>
                  </div>
                  <div v-if="item.id == editingTodos.id">
                    <div class="listContent">
                    <p class="font font-h5 totaltitle">TASK TITLE</p>
                    <form>
                      <label for=""></label>
                      <input type="text"
                      v-model="editingTitle" @keyup.enter="saveTodos(item)">
                    </form>
                    <p class="font font-h5 totaltitle">ESTIMATE TOMATO</p>
                    <div class="tomatoCount">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                      <img src="../assets/stylesheets/image/noneTomato.png" alt="" class="tomatoSingle">
                    </div>
                    </div>
                    <div class="listBtnGroup">
                      <div class="btn btn-secondary-light" @click="removeTodos(item.id)">DELETE</div>
                      <div class="btn btn-success-state" @click="doneTodos(item)">DONE</div>
                      <div class="btn btn-primary" @click="saveTodos(item)">SAVE</div>
                    </div>
                  </div>
                 
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
                  <div class="listTitle">
                    <div>
                      <i class="far fa-check-circle"></i> 
                      <p class="font-h4">Ring tone1</p>
                    </div>
                    <i class="far fa-play-circle"></i>
                  </div>
                  
                </li>
              </ul>
            </div>
          </div>
          <div class="toolbar">
              <ul>
                <li><i href="#" class="fas fa-plus-circle" @click="menu = 'addNew'"></i></li>
                <li><i href="#" class="fas fa-bars" @click="menu = 'list'"></i></li>
                <li><i href="#" class="fas fa-chart-bar" @click="menu = 'graph'"></i></li>
                <li><i href="#" class="fas fa-music"  @click="menu = 'music'"></i></li>
              </ul>
          </div>
        </div>
      </div>
      <!-- container -->
      <div class="container">
        <div class="container-inside" v-for="item in todos" :key="item.id">
          <h1 class="font-h2">{{item.title}}</h1>
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
      targetTime: 25*60, //換成秒
      minutes:'',
      seconds:'',
      now:0,
      percentage: 100,
      timer: '',
      count: '',
      menu: 'addTask',
      todos: [],
      newTodoTitle:'',
      visibility: 'todo',
      editingTodos: {},
      editingTitle: '',
      tomatoTodos: {}
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
    },
    addTodo () {
      let value = this.newTodoTitle.trim() //裁減前後空白
      if(!value){
        return
      }
      let timestamp = Math.floor(Date.now()) //時間戳記當id
      this.todos.push({
        id: timestamp,
        title: value,
        completed: false,
        tomatoCount: 1
      })
      this.newTodoTitle = ''
    },
    removeTodos (todo) {
      let newIndex = this.todos.findIndex((item) => {
        return todo.id === item.id
      })
      this.todos.splice(newIndex,1)
    },
    doneTodos (e) {
      this.todos.completed = e.complated
      e.completed = true
    },
    editTodos (e) {
      this.editingTodos = e
      this.editingTitle = e.title
    },
    saveTodos (item) {
      item.title = this.editingTitle
      this.editingTodos = {}
    },
    chosenTomato (e) {
      this.tomatoTodos = e
      // this.editingTitle = e.title
    }

  },
  components: {},
  computed: {
    circumference () {
      return 125*2*Math.PI
    },
    progress () {
      return this.circumference - this.circumference*this.percentage/100
    },
    filterTodos () {
      if(this.visibility === 'todo'){
        let newTodos = []
        this.todos.forEach((item)=>{
          if(!item.completed){
            newTodos.push(item)
          }
        })
        return newTodos
      }else if(this.visibility === 'done'){
        let newTodos = []
        this.todos.forEach((item)=>{
          if(item.completed){
            newTodos.push(item)
          }
        })
        return newTodos
      }
      return null
    }
  },
  created () {
    this.countTime()
  }
}
</script>
