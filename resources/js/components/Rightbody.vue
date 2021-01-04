<template>
  <div id="right">
    <h1>Development CRM</h1>

    <div class="horizontal">
      <img src="../images/horizontal.png" />
    </div>

    <p>
      Lorem ipsum, or lipsum as it is sometimes known, is dummy text used in
      laying out print, graphic or web designs. The passage is attributed to an
      unknown typesetter in the 15th century who is thought to have scrambled
      parts of Cicero's De Finibus Bonorum et Malorum for use in a type specimen
      book.
    </p>

    <div class="tasks">
      <div class="add-tasks">
        <h2>Today's Task</h2>
        <div class="add-action">
          <img src="../images/add.png" />
        </div>
      </div>

      <ul class="tasks-list">
        <li v-for="task in todaytasks" v-bind:key="task.id">
          <div class="info">
            <div class="left">
              <label class="myCheckbox">
                <input
                  type="checkbox"
                  name="test"
                  :checked="task.completed"
                  @change="updateTodayTasks(task.taskId)"
                />
                <span></span>
              </label>
              <h4>{{ task.title }}</h4>
            </div>
            <div class="right">
              <img src="../images/edit.png" />
              <img src="../images/del.png" @click="deleteTask(task.taskId)" />
              <button v-bind:class="{ inprogress: !todaytasks.waiting }">
                {{ task.approved ? "Approved" : "In Progress" }}
              </button>
            </div>
          </div>
        </li>
      </ul>
    </div>

    <div class="upcoming">
      <div class="add-tasks">
        <h2>Upcoming</h2>
        <div class="add-action">
          <img src="../images/add.png" alt="" />
        </div>
      </div>

      <form action="" @submit="addUpcomingTask">
        <input type="text" v-model="newTask" />
      </form>

      <ul class="tasks-list">
        <li v-for="upcomingtask in upcoming" v-bind:key="upcomingtask.id">
          <div class="info">
            <div class="left">
              <label class="myCheckbox">
                <input
                  type="checkbox"
                  name="test"
                  :checked="upcomingtask.completed"
                  @change="checkUpcoming(upcomingtask.taskId)"
                />
                <span></span>
              </label>
              <h4>{{ upcomingtask.title }}</h4>
            </div>
            <div class="right">
              <img src="../images/edit.png" />
              <img
                src="../images/del.png"
                @click="delUpcoming(upcomingtask.taskId)"
              />
              <button v-bind:class="{ inprogress: !upcoming.waiting }">
                Waiting
              </button>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todaytasks: [],
      upcoming: [],
      newTask: "",
    };
  },
  created() {
    this.fetchTodayTask();
    this.fetchUpcoming();
  },
  methods: {
    //** UPCOMING **//
    //?Fetching upcoming tasks
    fetchUpcoming() {
      fetch("/api/upcoming")
        .then((res) => res.json())
        .then(({ data }) => {
          this.upcoming = data;
        })
        .catch((err) => console.log(err));
    },
    //?Adding new Upcoming Tasks
    addUpcomingTask(e) {
      e.preventDefault();
      if (this.upcoming.length > 4) {
        alert("Please complete the upcoming task");
      } else {
        const newTask = {
          title: this.newTask,
          waiting: true,
          taskId: Math.random().toString(36).substring(7),
        };
        //?POST request
        fetch("api/upcoming", {
          method: "POST",
          headers: {
            "content-type": "application/json",
          },
          body: JSON.stringify(newTask),
        }).then(() => this.upcoming.push(newTask));

        //?Clear task fields
        this.newTask = "";
      }
    },
    //!Deleting Upcoming Tasks
    delUpcoming(taskId) {
      if (confirm("Are you sure to delete this upcoming task?")) {
        fetch(`/api/upcoming/${taskId}`, {
          method: "DELETE",
        })
          .then((response) => response.json())
          .then((data) => {
            this.upcoming = this.upcoming.filter(
              ({ taskId: id }) => id != taskId
            );
          })
          .catch((error) => console.error(error));
      }
    },

    //?Check upcoming task
    checkUpcoming(taskId) {
      if (this.todaytasks.length > 4) {
        alert("Sorry complete existing task");
        window.location.href = "/";
      } else {
        this.addDailyTask(taskId);

        //? Delete from this task from db
        fetch(`/api/upcoming/${taskId}`, {
          method: "DELETE",
        }).then(
          () =>
            (this.upcoming = this.upcoming.filter(
              ({ taskId: id }) => id !== taskId
            ))
        );
      }
    },

    //** TODAY'S TASKS **//
    //?Fetching today tasks
    fetchTodayTask() {
      fetch("api/dailytask")
        .then((response) => response.json())
        .then(({ data }) => {
          this.todaytasks = data;
        })
        .catch((err) => console.error(err));
    },

    //?Adding daily tasks
    addDailyTask(taskId) {
      //? GET task
      const task = this.upcoming.filter(({ taskId: id }) => id == taskId)[0];

      //?POST request
      fetch("api/dailytask", {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(task),
      })
        .then(() => this.todaytasks.unshift(task))
        .catch((error) => console.error(error));
    },

    //?Update today's tasks
    updateTodayTasks(taskId) {
      if (confirm("Is this task complete?")) {
        fetch(`/api/dailytask/${taskId}`, { method: "DELETE" })
          .then(() => {})
          .then(
            () =>
              (this.todaytasks = this.todaytasks.filter(
                ({ taskId: id }) => id !== taskId
              ))
          )
          .catch((error) => console.error(error));
      }
    },

    //?Delete today's task
    deleteTask(taskId) {
      if (confirm("Are you sure you want to delete?")) {
        fetch(`/api/dailytask/${taskId}`, {
          method: "DELETE",
        })
          .then((response) => response.json())
          .then(
            () =>
              (this.todaytasks = this.todaytasks.filter(
                ({ taskId: id }) => id !== taskId
              ))
          )
          .catch((err) => console.error(err));
      }
    },
  },
};
</script>

<style>
</style>