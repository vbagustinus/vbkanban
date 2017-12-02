<template>
   <div class='container'>
    <div class='row'>
      <div class='col-md-12'>
        <h1 style='color:#000 !important;'>KANBAN-APP</h1>
        <hr>
        <!-- <div class='col-md-10'>
        </div> -->
        <div class='col-md-offset-10 col-md-1'>
          <!-- Trigger the modal with a button -->
          <button type='button' style='margin-left: 80%;'class='btn btn-info btn-lg' data-toggle='modal' data-target='#myModal'>+ ADD</button>
          <h1></h1>
        </div>
        <!-- task -->
        <div class='col-md-3'>
          <div class='row'>
            <div class='col-sm-12'>
              <div class='thumbnail' >
                <h1 style='background-color:red;'>Back Log</h1>
                <div class='thumbnail' v-for='task in showtasks' v-if='task.status == 0'>
                  <div class='caption'>
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click='removeTask(task)' type='button'>Remove</button>
                      <button @click='toDo(task)' type='button'>Todo</button>
                    </div>
                  </div>
                  <!-- modal DETAIl -->
                </div>

              </div>
            </div>
          </div>
        </div>
        <!-- TODO -->
        <div class='col-md-3'>
          <div class='row'>
            <div class='col-sm-12'>
              <div class='thumbnail'>
                <h1 style='background-color:orange;'>TO-DO</h1>
                <div class='thumbnail' v-for='task in showtasks' v-if='task.status == 1'>
                  <div class='caption'>
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click='backLog(task)' type='button'>Back Log</button>
                      <button @click='removeTask(task)' type='button'>Remove</button>
                      <button @click='doIng(task)' type='button'>Doing</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DOING -->
        <div class='col-md-3'>
          <div class='row'>
            <div class='col-sm-12'>
              <div class='thumbnail'>
                <h1 style='background-color:#b3f442;'>DOING</h1>
                <div class='thumbnail' v-for='task in showtasks' v-if='task.status == 2'>
                  <div class='caption'>
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click='toDo(task)' type='button'>Todo</button>
                      <button @click='removeTask(task)' type='button'>Remove</button>
                      <button @click='doNe(task)' type='button'>Done</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DONE -->
        <div class='col-md-3'>
          <div class='row'>
            <div class='col-sm-12'>
              <div class='thumbnail'>
                <h1 style='background-color:green;'>DONE</h1>
                <div class='thumbnail' v-for='task in showtasks' v-if='task.status == 3'>
                  <div class='caption'>
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Desc: {{ task.desc }} </p>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click='doIng(task)' type='button'>Doing</button>
                      <button @click='removeTask(task)' type='button'>Remove</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

    </div>
    <!-- Modal Add-->
    <div class='modal fade' id='myModal' role='dialog'>
      <div class='modal-dialog'>
        <!-- Modal content-->
        <div class='modal-content'>
          <div class='modal-header'>
            <h4 class='modal-title'>Input Task</h4>
          </div>
          <div class='modal-body'>
            <form>
              <div class='form-group'>
                <label for='Title'>Title:</label>
                <input v-model='inputTask.title' type='text' class='form-control' id='title'>
              </div>
              <div class='form-group'>
                <label for='Description'>Description:</label>
                <textarea v-model='inputTask.desc' class='form-control' rows='5' id='description'></textarea>
              </div>
              <div class='form-group'>
                <label for='Point'>Point:</label>
                <input v-model='inputTask.point' type='text' class='form-control' id='point'>
              </div>
              <div class='form-group'>
                <label for='AssignTo'>Assign To:</label>
                <input v-model='inputTask.assignTo' type='text' class='form-control' id='assign'>
              </div>
            </form>
          </div>
          <div class='modal-footer'>
            <button type='button' class='btn btn-default' data-dismiss='modal'>Close</button>
            <button @click='saveTask' type='button' class='btn btn-default' data-dismiss='modal'>Add Task</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import * as firebase from 'firebase'

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
  firebase: {
    showtasks: db
  },
  methods: {
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
h1, h2 {
  font-weight: normal;
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
</style>
