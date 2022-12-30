<template>
  <div v-if="!loading">
    <v-snackbar v-model="snackDialog.visible" timeout="2000">
      {{ snackDialog.message }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="blue"
          text
          v-bind="attrs"
          @click="snackDialog.visible = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
    <v-dialog v-model="newTaskDialog" width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Create A New Task</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row class="mt-3">
              <v-col cols="12" sm="12" md="12" class="ma-0 pa-0">
                <v-text-field
                  solo
                  label="Task Title*"
                  v-model="newTaskForm.title"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="12" md="12" class="ma-0 pa-0">
                <v-textarea
                  solo
                  label="Task Description"
                  v-model="newTaskForm.description"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="newTaskDialog = false">
            Close
          </v-btn>
          <v-btn color="blue darken-1" text @click="createNewTask">
            Create
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Edit task dialog -->

    <v-dialog v-model="editTaskDialog" width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Edit Task</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row class="mt-3">
              <v-col cols="12" sm="12" md="12" class="ma-0 pa-0">
                <v-text-field
                  solo
                  label="Task Title*"
                  v-model="editTaskForm.title"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="12" md="12" class="ma-0 pa-0">
                <v-textarea
                  solo
                  label="Task Description"
                  v-model="editTaskForm.description"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-container>
          <small>*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="editTaskDialog = false">
            Close
          </v-btn>
          <v-btn color="blue darken-1" text @click="editTask"> Edit </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <div
      class="d-flex text--disabled pa-2 grey lighten-3 rounded"
      style="width: 100%"
    >
      <v-icon class="mb-1 mr-2">{{ selectedPage.icon }}</v-icon>
      <h2>{{ selectedPage.text }}</h2>
      <v-spacer />
      <v-btn color="primary" class="black--text" @click="newTaskDialog = true"
        ><v-icon>mdi-plus</v-icon> Add New Task</v-btn
      >
    </div>
    <v-row>
      <v-col cols="4" class="mt-5" v-for="(task, i) in tasks" :key="i">
        <v-card color="secondary">
          <v-card-title class="dflex">
            <h5 class="text-h5">{{ task.title }}</h5>
            <span class="text-caption ml-auto">
              <v-btn
                icon
                x-small
                @click="(editTaskDialog = true), (selectedTask = task)"
                ><v-icon>mdi-note-edit</v-icon></v-btn
              >
            </span>
          </v-card-title>
          <v-card-subtitle>{{
            $moment(task.created).format("DD-MM-YYYY HH:mm")
          }}</v-card-subtitle>

          <v-card-text>{{ task.description }}</v-card-text>

          <v-card-actions class="d-flex flex-row justify-end">
            <div class="actionBox" v-for="(action, i) in taskActions" :key="i">
              <v-btn
                class="ml-2"
                small
                @click="changeStatusTask(task, action.value)"
                v-if="action.value !== selectedPage.status"
                ><v-icon small>{{ action.icon }}</v-icon
                >{{ action.text }}</v-btn
              >
            </div>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <div class="text-center mt-12">
      <v-pagination
        v-model="activePage"
        :length="Math.ceil(taskCount / 9)"
      ></v-pagination>
    </div>
  </div>
  <div v-else>
    <h2>Loading...</h2>
  </div>
</template>

<script>
import { computed } from "vue";
export default {
  name: "Board",
  props: {
    selectedPage: {
      type: Object,
    },
    taskCount: {
      type: Number,
    },
  },
  watch: {
    selectedPage: async function (val) {
      this.tasks = [];
      await this.fetchTasks(val);
    },
    activePage: async function () {
      this.tasks = [];
      await this.fetchTasks(this.selectedPage);
    },
  },
  data() {
    return {
      tasks: [],
      loading: true,
      selectedTask: {},
      activePage: 1,
      editTaskDialog: false,
      editTaskForm: {
        title: "",
        description: "",
      },
      newTaskDialog: false,
      newTaskForm: {
        title: "",
        description: "",
      },
      taskActions: [
        {
          text: "Todo",
          value: "todo",
          icon: "mdi-play",
        },
        {
          text: "Completed",
          value: "completed",
          icon: "mdi-check",
        },
        {
          text: "Ongoing",
          value: "ongoing",
          icon: "mdi-play",
        },
      ],
      snackDialog: {
        visible: false,
        message: "",
      },
    };
  },

  async created() {
    await this.fetchTasks(this.selectedPage);
  },
  methods: {
    async fetchTasks(val) {
      await fetch(
        "http://localhost:3000/get-tasks/" + val.status + "/" + this.activePage,
        {
          headers: { "Content-Type": "application/json" },
          credentials: "include",
        }
      )
        .then((response) => response.json())
        .then((json) => (this.tasks = json));

      this.loading = false;
    },

    async createNewTask() {
      try {
        let response = await fetch("http://localhost:3000/create-tasks", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          credentials: "include",
          body: JSON.stringify(this.newTaskForm),
        });
        if (response.status !== 200) {
          this.snackDialog = {
            visible: true,
            message: "An error occured. Please check your fields.",
          };
        }
        (this.newTaskForm = {
          title: "",
          description: "",
        }),
          this.taskCount++;
        await this.fetchTasks(this.selectedPage);
      } catch {}
      this.newTaskDialog = false;
    },

    async editTask() {
      try {
        let response = await fetch(
          "http://localhost:3000/edit-tasks/" + this.selectedTask._id,
          {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            credentials: "include",
            body: JSON.stringify({
              title: this.editTaskForm.title || this.selectedTask.title,
              description:
                this.editTaskForm.description || this.selectedTask.description,
              status: this.editTaskForm.status || this.selectedTask.status,
            }),
          }
        );

        if (response.status === 200) {
          this.snackDialog = {
            visible: true,
            message: "Your task edited.",
          };
          this.editTaskDialog = false;
          this.editTaskForm = {};
          this.fetchTasks(this.selectedPage);
        } else {
          this.snackDialog = {
            visible: true,
            message: "An error occured.",
          };
        }
      } catch {
        this.snackDialog = {
          visible: true,
          message: "An error occured.",
        };
      }
    },

    async changeStatusTask(task, newStatus) {
      try {
        let response = await fetch(
          "http://localhost:3000/edit-tasks/" + task._id,
          {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            credentials: "include",
            body: JSON.stringify({
              title: task.title,
              description: task.description,
              status: newStatus,
            }),
          }
        );

        if (response.status === 200) {
          this.snackDialog = {
            visible: true,
            message: "Your task status changed.",
          };
          this.fetchTasks(this.selectedPage);
        } else {
          this.snackDialog = {
            visible: true,
            message: "An error occured.",
          };
        }
      } catch {
        this.snackDialog = {
          visible: true,
          message: "An error occured.",
        };
      }
    },
  },
};
</script>
<style scoped></style>
