<template>
   <div>
     <section class="hero is-light is-small is-bold">
        <div class="hero-body">
          <div class="container">
            <nav class="navbar is-transparent">
              <div class="navbar-brand">
                  <h1 class="title"> VB Kanban App</h1>
                <div class="navbar-burger burger" data-target="navbarExampleTransparentExample">
                  <span></span>
                  <span></span>
                  <span></span>
                </div>
              </div>
              <div id="navbarExampleTransparentExample" class="navbar-menu">
                <div class="navbar-end">
                  <div class="navbar-item">
                    <div class="field is-grouped">
                      <p class="control">
                        <a class="button is-info" @click="isComponentModalActive = true">
                          <span class="icon">
                            <i class="fa fa-pencil-square-o">+</i>
                          </span>
                          <span>Add Task</span>
                        </a>
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </nav>
            <div class="columns">
              <div class="column">
                  <h1 style='background-color:#404040;color:#fff;'>Back Log</h1>
                  <br/>
                <div class='thumbnail' v-for='task in showtasks' :key='task.title' v-if='task.status == 0'>
                  <div class='caption'>
                    <div>
                      <h3 class="subtitle"><b>{{ task.title }}</b></h3>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button class="button is-danger" @click='removeTask(task)' type='button'>Remove</button>
                      <button class="button is-success" @click='toDo(task)' type='button'>Todo</button>
                      <hr><hr>
                    </div>
                  </div>
                </div>
              </div>
              <div class="column">
                <h1 style='background-color:#B71427;color:#fff;'>TO-DO</h1>
                <br/>
                <div class='thumbnail' v-for='task in showtasks' :key='task.title' v-if='task.status == 1'>
                  <div class='caption'>
                    <div>
                      <h3 class="subtitle"><b>{{ task.title }}</b></h3>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button class="button is-warning" @click='backLog(task)' type='button'>Back Log</button>
                      <button class="button is-danger" @click='removeTask(task)' type='button'>Remove</button>
                      <button class="button is-success" @click='doIng(task)' type='button'>Doing</button>
                      <hr><hr>
                    </div>
                  </div>
                </div>
              </div>
              <div class="column">
                <h1 style='background-color:#FFE658;'>DOING</h1>
                <br/>
                <div class='thumbnail' v-for='task in showtasks' :key='task.title' v-if='task.status == 2'>
                  <div class='caption'>
                    <div>
                      <h3 class="subtitle"><b>{{ task.title }}</b></h3>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button class="button is-warning" @click='toDo(task)' type='button'>Todo</button>
                      <button class="button is-danger" @click='removeTask(task)' type='button'>Remove</button>
                      <button class="button is-success" @click='doNe(task)' type='button'>Done</button>
                      <hr><hr>
                    </div>
                  </div>
                </div>
              </div>
              <div class="column">
                <h1 style='background-color:#6DBDD6;color:#fff;'>DONE</h1>
                <br/>
                <div class='is-info' v-for='task in showtasks' :key='task.title' v-if='task.status == 3'>
                  <div class='caption'>
                    <div>
                      <h3 class="subtitle"><b>{{ task.title }}</b></h3>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button class="button is-warning" @click='doIng(task)' type='button'>Doing</button>
                      <button class="button is-danger" @click='removeTask(task)' type='button'>Remove</button>
                      <hr><hr>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <b-modal :active.sync="isComponentModalActive" has-modal-card class="huruf">
        <!-- Modal content-->
        <b-field>
            <b-input 
              required
              placeholder="Title" 
              type="text"
              v-model='inputTask.title'>
            </b-input>
        </b-field>
        <b-field>
            <b-input 
              required
              v-model='inputTask.desc'
              type="textarea"
              minlength="5"
              maxlength="100"
              placeholder="Maxlength automatically counts characters (100)">
            </b-input>
        </b-field>
        <b-field>
            <b-input 
              required
              v-model='inputTask.point'
              placeholder="Point"
              type="number"
              min="0"
              max="100">
            </b-input>
        </b-field>
         <b-field>
            <b-input 
              required
              type="text"
              v-model='inputTask.assignTo'
              placeholder="Assign To">
            </b-input>
        </b-field>
        <button type='button' class="button is-light" @click='close'>Close</button>
        <button @click='saveTask' type='button' class="button is-info" data-dismiss='close'>Add Task</button>
      </b-modal>
  </div>
</template>

<script>
import * as firebase from 'firebase'
import InputForm from '@/components/InputForm'
// Initialize Firebase
var config = {
  databaseURL: 'https://vbkanban.firebaseio.com',
  projectId: 'vbkanban'
}

const firebaseApp = firebase.initializeApp(config)
const db = firebaseApp.database().ref('tasks')

export default {
  name: 'Kanban',
  data () {
    return {
      isComponentModalActive: false,
      newTaskModal: false,
      detailTaskModal: false,
      inputTask: {
        title: null,
        desc: null,
        point: null,
        assignTo: null,
        status: 0
      },
      currentTask: null
    }
  },
  components: {
    InputForm
  },
  firebase: {
    showtasks: db
  },
  methods: {
    close () {
      this.isComponentModalActive = false
    },
    saveTask () {
      db.push(this.inputTask)
      this.inputTask.title = null
      this.inputTask.desc = null
      this.inputTask.point = null
      this.inputTask.assignTo = null
      this.inputTask.status = 0
    },
    toDo (task) {
      console.log('TODO')
      let data = db.child(task['.key'])
      data.update({
        'status': 1
      })
    },
    doIng (task) {
      console.log('DOING')
      let data = db.child(task['.key'])
      data.update({
        'status': 2
      })
    },
    doNe (task) {
      console.log('DONE')
      let data = db.child(task['.key'])
      data.update({
        'status': 3
      })
    },
    backLog (task) {
      console.log('BACKLOG')
      let data = db.child(task['.key'])
      data.update({
        'status': 0
      })
    },
    removeTask (task) {
      console.log('REMOVE')
      db.child(task['.key']).remove()
    }
  }
}
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Macondo+Swash+Caps|Permanent+Marker');

* {
  font-family: 'Macondo Swash Caps', cursive!important;
  
}
body {
  padding: 0;
  margin: 0;
  font-family: 'Macondo Swash Caps', cursive!important;
}
.input, .textarea{
  font-family: 'Macondo Swash Caps', cursive!important;
  font-size: 12px;
}
h1, h2 {
  font-weight: normal;
  font-family: 'Permanent Marker', cursive!important;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
