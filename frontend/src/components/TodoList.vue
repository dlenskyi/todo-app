<template>
  <div class="hello">
    <h1 class="title">TODO List</h1>

    <hr>

    <div class=" block-top columns">
      <div class="column is-one-third is-offset-one-third">
        <form @submit.prevent="addTask">

          <div class="field">
            <label class="label">Description</label>
            <div class="control">
              <input class="input" type="text" v-model="description">
            </div>
          </div>

          <div class="field is-grouped is-grouped-centered">
            <div class="control">
              <button class="button is-link">Add task!</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <!-- <hr> -->

    <div class=" block-bot columns">
      <div class="column is-one-third is-offset-one-third">
        <h2 class="subtitle"><b>Tasks</b></h2>
        <div class="todo">
          <div class="card finished" v-for="task in tasks" v-if="task.status == 1">
            <div class="card-content">
              <div class="content info">
                {{ task.description }}
              </div>
            </div>

            <footer class="card-footer">
              <!-- When you click 'Done', setStatus will be called and the task's id will be passed as a parameter to the function -->
              <a class="card-footer-item button is-light" @click="resetStatus(task.id)">Undone</a>
              <a class="card-footer-item button button is-light" @click="delTask(task.id)">Delete</a>
            </footer>
          </div>
          <div class="card not-finished" v-else>
            <div class="card-content">
              <div class="content info">
                {{ task.description }}
              </div>
            </div>

            <footer class="card-footer">
              <a class="card-footer-item button is-primary" @click="setStatus(task.id)">
                Done</a>
              <a class="card-footer-item button is-danger" @click="delTask(task.id)">
                Delete</a>
            </footer>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'TodoList',
  data () {
    return {
      tasks: [],
      description: '',
      status: 0
    }
  },
  mounted () {
    this.getTasks();
  },
  methods: {

    // This func launches every time page reloads and outputs all tasks
    getTasks: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/tasks',
        auth: {
          username: 'root',
          password: '1234'
        }
      }).then(response => this.tasks = response.data);
    },

    // Func which adds new task to db
    addTask: function () {
      if (this.description) {
        axios({
          method: 'post',
          url: 'http://127.0.0.1:8000/tasks/',
          data: {
            description: this.description,
            status: this.status
          },
          auth: {
            username: 'root',
            password: '1234'
          }
        }).then((response) => {
          var newTask = {'id': response.data.id, 'description': this.description, 'status': parseInt(this.status)}

          this.tasks.push(newTask)

          this.description = ''
          this.status = 0
        })
        .catch((error) => {
          console.log(error);
        });
      }
    },

    // This func sets task as done
    setStatus: function (task_id) {
      var description = '';

      for (var i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].id === task_id) {
          this.tasks[i].status = 1;
          description = this.tasks[i].description;
          break
        }
      }
      axios({
        method:'put',
        url: 'http://127.0.0.1:8000/tasks/' + task_id + '/',
        headers: {
          'Content-Type': 'application/json'
        },
        data: {
          description: description,
          status: 1
        },
        auth: {
          username: 'root',
          password: '1234'
        }
      })
    },
    // Opposite to previous func
    resetStatus: function (task_id) {
      var description = '';

      for (var i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].id === task_id) {
          this.tasks[i].status = 0;
          description = this.tasks[i].description;
          break
        }
      }
      axios({
        method:'put',
        url: 'http://127.0.0.1:8000/tasks/' + task_id + '/',
        headers: {
          'Content-Type': 'application/json'
        },
        data: {
          description: description,
          status: 0
        },
        auth: {
          username: 'root',
          password: '1234'
        }
      })
    },
    // It deleted task from db
    delTask: function (task_id) {
      for (var i = 0; i < this.tasks.length; i++) {
        if (this.tasks[i].id === task_id) {
          this.tasks.splice(i, 1);
          break
        }
      }
      axios({
        method: 'delete',
        url: 'http://127.0.0.1:8000/tasks/' + task_id,
        auth: {
          username: 'root',
          password: '1234'
        }
    }).then(response => {
      this.tasks.splice(task_id, 1);
    })
    }
  }
}
</script>

<style scoped>

  h2 {
    font-size: 1.5em;
  }

  .card {
    margin-bottom: 25px;
  }

  .done {
    opacity: 0.3;
  }

  .finished {
    background-color: silver;
  }

  .not-finished {
    background-color: #E6CFA8;
  }

  .block-top {
    background-color: #F3F3F3;
    border-top: 2px solid #AFCA88;
    border-left: 2px solid #AFCA88;
    border-right: 2px solid #AFCA88;
  }

  .info {
    word-wrap:break-word;
  }

  .block-bot {
    background-color: #F3F3F3;
    border: 2px solid #AFCA88;
  }
</style>
