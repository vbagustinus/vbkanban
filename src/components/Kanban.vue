<template>
   <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h1 style="color:#000 !important;">KANBAN-APP</h1>
        <hr>
        <!-- <div class="col-md-10">
        </div> -->
        <div class="col-md-offset-10 col-md-1">
          <!-- Trigger the modal with a button -->
          <button type="button" style="margin-left: 80%;"class="btn btn-info btn-lg" data-toggle="modal" data-target="#myModal">+ ADD</button>
          <h1></h1>
        </div>
        <!-- task -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail" >
                <h1 style="background-color:red;">task</h1>

                <div class="thumbnail" v-for="task in showtasks" v-if="task.status == 0">
                  <div class="caption">
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <!-- <button @click="showDetail()" type="button">Show Detail</button> -->
                      <button @click="showDetail(task)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                  <!-- modal DETAIl -->


                </div>

              </div>
            </div>
          </div>
        </div>
        <!-- TODO -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:orange;">TO-DO</h1>
                <div class="thumbnail" v-for="task in showtasks" v-if="task.status == 1">
                  <div class="caption">
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click="showDetail(task)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DOING -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:#b3f442;">DOING</h1>
                <div class="thumbnail" v-for="task in showtasks" v-if="task.status == 2">
                  <div class="caption">
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click="showDetail(task)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!-- DONE -->
        <div class="col-md-3">
          <div class="row">
            <div class="col-sm-12">
              <div class="thumbnail">
                <h1 style="background-color:green;">DONE</h1>
                <div class="thumbnail" v-for="task in showtasks" v-if="task.status == 3">
                  <div class="caption">
                    <div>
                      <h3><b>{{ task.title }}</b></h3>
                      <hr>
                      <p>Point: {{ task.point }} </p>
                      <p>Assigned To: {{ task.assignTo }} </p>
                      <button @click="showDetail(task)" type="button" class="btn btn-info btn-lg" data-toggle="modal" data-target="#modalDetail">Show Detail</button>
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
    <div class="modal fade" id="myModal" role="dialog">
      <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Modal Kanban</h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title" style="text-align='left'">Title:</label>
                <input v-model="addTasks.title" type="text" class="form-control" id="title">
              </div>
              <div class="form-group">
                <label for="Description">Description:</label>
                <textarea v-model="addTasks.description" class="form-control" rows="5" id="description"></textarea>
              </div>
              <div class="form-group">
                <label for="Point">Point:</label>
                <input v-model="addTasks.point" type="text" class="form-control" id="point">
              </div>
              <div class="form-group">
                <label for="AssignTo">Assign To:</label>
                <input v-model="addTasks.assignTo" type="text" class="form-control" id="assign">
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
            <button @click="addTask" type="button" class="btn btn-default" data-dismiss="modal">Add Task</button>
          </div>
        </div>

      </div>
    </div>

    <!-- modal show detail -->
    <div v-if="currentTask !== null" class="modal fade" id="modalDetail" role="dialog">
      <div class="modal-dialog">
        <!-- Modal content-->
        <div class="modal-content" style="text-align:'left'" >
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal">&times;</button>
            <h4 class="modal-title">Detail Task : {{ currentTask.title }} for {{ currentTask.assignTo }} </h4>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="Title" style="text-align='left'">Task Description: {{ currentTask.description }}</label>
                <p> Point : {{ currentTask.point }} </p>
                <p> Status : {{ currentTask.status }}</p>
              </div>
            </form>
          </div>
          <div class="modal-footer" >
            <button v-if="currentTask.status == 'todo'" @click="task(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">task</button>
            <button @click="removeTask(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DELETE</button>
            <button v-if="currentTask.status == 'doing' || currentTask.status == 'task'" @click="todo(currentTask)" type="button" class="btn btn-default" data-dismiss="modal">TO-DO</button>
            <button v-if="currentTask.status == 'todo'  || currentTask.status == 'done'" @click="doing(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DOING</button>
            <button v-if="currentTask.status == 'doing'" @click="done(currentTask)" type="button" class="btn btn-default danger" data-dismiss="modal">DONE</button>

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
  apiKey: 'AIzaSyCBRDUAkXB6G5VtiFzwP0F-J4a8EEETTrM',
  authDomain: 'vbkanban.firebaseapp.com',
  databaseURL: 'https://vbkanban.firebaseio.com',
  projectId: 'vbkanban',
  storageBucket: 'vbkanban.appspot.com',
  messagingSenderId: '294358815008'
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
        assignedTo: null,
        status: 0
      }
    }
  },
  firebase: {
    showtasks: db
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
