<template>
  <div class="hello">

    <div class="columns">
      <div class="column is-one-third is-offset-one-third">

        <!-- Title and delete button -->
        <div class="title content">
          <p>
            TODO list
            <span @click="delTask" class="icon is-small is-right">
                <i class="fas fa-trash"></i>
            </span>
        </p>
        </div>

        <!-- Displays done and undone tasks -->
        <div>

          <!-- Case if task is done -->
          <div v-for="task in tasks" v-if="task.status == 1">
            <div class="card-content">
              <div class="content info">
                <span @click="resetStatus(task.id)" class="icon checkboxes">
                  <font-awesome-icon :icon="['fas', 'check-circle']" size="2x" />
                </span>
              </div>
                <span class="task-description">
                  {{ task.description }}
                </span>
            </div>
          </div>

          <!-- Case if task is undone -->
          <div class="" v-else>
            <div class="card-content">
              <div class="content info">
                <span @click="setStatus(task.id)" class="icon checkboxes">
                  <font-awesome-icon :icon="['far', 'circle']" size="2x" />
                </span>
              </div>
                <span class="task-description">
                  {{ task.description }}
                </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Field for adding tasks -->
    <div class="columns">
      <div class="column is-one-third is-offset-one-third">
        <form @submit.prevent="addTask">
          <div class="field is-horizontal">
              <div class="field-label is-normal">
                <span @click="addTask" class="icon plus-icon is-small">
                  <font-awesome-icon :icon="['fas', 'plus']" />
                </span>
              </div>
              <div class="field-body">
                <p class="control">
                  <input class="input" placeholder="Enter new task here!" type="text" v-model="description">
                </p>
              </div>
          </div>
        </form>
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

    // It deletes all selected tasks from db
    delTask: function () {
      var i = this.tasks.length;
      while (i--) {
        if (this.tasks[i].status === 1) {
          axios({
            method: 'delete',
            url: 'http://127.0.0.1:8000/tasks/' + this.tasks[i].id,
            auth: {
              username: 'root',
              password: '1234'
            }
          })
          this.tasks.splice(i, 1);
        }
      }
    },
  }
}
</script>

<style scoped>

  .title>p {
    background-color: #EEEEEE;
    color: #6C6C6C;
    padding: 8px 0 8px 8px;
    font-size: 1.3rem;
    text-align: left;
    font-weight: 100;
    border-radius: 3%;
  }

  .title .icon {
    position: relative;
    float: right;
    right: 3%;
    padding-top: 5px;
  }

  .icon {
    cursor: pointer;
  }

  .fa-trash {
    color: black;
  }

  .checkboxes {
    float: left;
    margin-right: 5px;

  }

  .plus-icon {
    position: relative;
    top: 3px;
  }

  .task-description {
    float: left;
    font-size: 1.7em;
    margin-left: 15%;
    position: relative;
    bottom: 30px;
    word-wrap:break-word;
    text-align: left;
    max-width: 85%;
  }

</style>
