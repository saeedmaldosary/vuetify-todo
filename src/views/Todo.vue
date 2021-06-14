<template>
  <v-container>
    <v-row align="center" justify="center" >
      <v-col cols="12" md="6" lg="8">
        <div class="home justify-center">
          <!-- progress-circular -->
          <!-- <div class="text-center">
            <v-text-field value="completedTasks" readonly></v-text-field>
            <v-progress-circular
              :rotate="-90"
              :size="100"
              :width="15"
              class="mx-6"
              :value="completedTasksProgress"
              color="primary"
            >
              {{ completedTasks }}
            </v-progress-circular>
            <v-divider vertical></v-divider>
            <v-progress-circular
              :rotate="-90"
              :size="100"
              :width="15"
              class="mx-6"
              :value="remainingTasksProgress"
              color="teal"
            >
              {{ remainingTasks }}
            </v-progress-circular>
          </div> -->
          <!-- End progress-circular -->
          <!-- progress-linear -->
          <template>
            <v-container fluid>
              <v-text-field
                v-model="value"
                color="cyan darken"
                label="Remaining Tasks"
                loading
              >
                <template v-slot:progress>
                  <v-progress-linear
                    :value="remainingTasksProgress"
                    color="amber"
                    absolute
                    height="25"
                  >
                    <strong>{{ Math.ceil(remainingTasksProgress) }}%</strong>
                  </v-progress-linear>
                </template>
              </v-text-field>
              <v-text-field
                v-model="value"
                color="cyan darken"
                label="Completed Tasks"
                loading
              >
                <template v-slot:progress>
                  <v-progress-linear
                    :value="completedTasksProgress"
                    color="green"
                    absolute
                    height="25"
                  >
                    <strong>{{ Math.ceil(completedTasksProgress) }}%</strong>
                  </v-progress-linear>
                </template>
              </v-text-field>
            </v-container>
          </template>

          <v-divider class="mx-5"></v-divider>
          <!-- End progress-linear -->

          <v-text-field
            outlined
            v-model="newTaskTitle"
            class="pa-3"
            label="Add task"
            hide-details
            @click:append="addTask"
            @keyup.enter="addTask"
            append-icon="mdi-plus-circle-outline"
          />
          <v-list subheader two-line flat class="pt-0">
            <div v-for="task in filteredTasks">
              <v-list-item
                @click="doneTask(task.id)"
                :class="{ 'blue lighten-5': task.done }"
              >
                <template>
                  <v-list-item-action>
                    <v-checkbox
                      :input-value="task.done"
                      color="primary"
                    ></v-checkbox>
                  </v-list-item-action>

                  <v-list-item-content>
                    <v-list-item-title
                      :class="{ 'text-decoration-line-through': task.done }"
                      >{{ task.title }}</v-list-item-title
                    >
                  </v-list-item-content>
                  <v-list-item-action>
                    <v-btn icon @click.stop="deleteTask(task.id)">
                      <v-icon color="primary lighten-1">mdi-delete</v-icon>
                    </v-btn>
                  </v-list-item-action>
                </template>
              </v-list-item>
              <v-divider></v-divider>
            </div>
          </v-list>
        </div>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" :multi-line="multiLine">
      {{ text }}

      <template v-slot:action="{ attrs }">
        <v-btn color="blue" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      interval: {},
      value: "",
      custom: true,
      multiLine: true,
      snackbar: false,
      text: `I'm a multi-line snackbar.`,
      newTaskTitle: null,
      tasks: [
        {
          id: 1,
          title: "Wake up",
          done: false,
        },
        {
          id: 2,
          title: "Gym",
          done: false,
        },
        {
          id: 3,
          title: "Work",
          done: false,
        },
      ],
    };
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },
  props: {
    search: {
      type: String,
      required: true,
    },
  }, // End of props
  computed: {
    filteredTasks() {
      return this.tasks.filter((task) => {
        return task.title.match(this.search);
      });
    },
    completedTasks() {
      return this.tasks.filter((task) => task.done).length;
    },
    completedTasksProgress() {
      return (100 / this.tasks.length) * this.completedTasks;
    },

    remainingTasks() {
      return this.tasks.length - this.completedTasks;
    },
    remainingTasksProgress() {
      return 100 - (100 / this.tasks.length) * this.completedTasks;
    },
  },
  methods: {
    doneTask(id) {
      this.text = this.tasks.find((x) => x.id === id).title + " task is done";
      this.snackbar = true;
      let task = this.tasks.filter((task) => task.id == id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      this.text =
        this.tasks.find((x) => x.id === id).title + " task has been deleted";
      this.snackbar = true;
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    addTask() {
      if (this.newTaskTitle) {
        let newTask = {
          id: Date.now(),
          title: this.newTaskTitle,
          done: false,
        };
        this.snackbar = true;
        this.text = this.newTaskTitle + " Task has been added";
        this.tasks.push(newTask);
        this.newTaskTitle = "";
      } else {
        this.snackbar = true;
        this.text = "Enter a task to add it";
      }
    },
  },
};
</script>
