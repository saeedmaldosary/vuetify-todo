<template>
  <div class="home ">
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
              <v-checkbox :input-value="task.done" color="primary"></v-checkbox>
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
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
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
  },
  methods: {
    doneTask(id) {
      let task = this.tasks.filter((task) => task.id == id)[0];
      task.done = !task.done;
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    addTask() {
      if (this.newTaskTitle != "") {
        let newTask = {
          id: Date.now(),
          title: this.newTaskTitle,
          done: false,
        };
        this.tasks.push(newTask);
        this.newTaskTitle = "";
      }
    },
  },
};
</script>
