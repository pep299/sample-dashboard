<template>
  <v-container class="container mt-10">
    <v-text-field
      v-model="task"
      label="What are you working on?"
      solo
      @keydown.enter="create"
    >
      <!-- <v-fade-transition v-slot:append>
        <v-icon
          v-if="task"
          @click="create"
        >
          add_circle
        </v-icon>
      </v-fade-transition> -->
    </v-text-field>

    <h2 class="display-1 success--text pl-4">
      Tasks:
      <v-fade-transition leave-absolute>
        <span :key="`tasks-${tasks.length}`">
          {{ tasks.length }}
        </span>
      </v-fade-transition>
    </h2>

    <v-divider class="mt-4"></v-divider>

    <v-row
      class="my-1"
      align="center"
    >
      <strong class="mx-4 info--text text--darken-2">
        Remaining: {{ remainingTasks }}
      </strong>
      <v-spacer></v-spacer>

      <v-divider vertical></v-divider>

      <v-spacer></v-spacer>
      <strong class="mx-4 success--text text--darken-2">
        Completed: {{ completedTasks }}
      </strong>
      <v-progress-circular
        :rotate=270
        :value="progress"
        class="mx-4"
      ></v-progress-circular>
      <v-spacer></v-spacer>

      <v-divider vertical></v-divider>

      <v-spacer></v-spacer>
      <v-btn v-if="!isSwap" icon class="mx-4" @click="startSwapTasks">
        <v-icon>mdi-swap-vertical</v-icon>
      </v-btn>
      <v-btn v-else icon class="mx-4" @click="endSwapTasks">
        <v-icon color="green">mdi-check-bold</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
    </v-row>

    <v-divider class="mb-4"></v-divider>

    <v-card v-if="tasks.length > 0">
      <v-list v-if="isSwap" class="py-0">
        <draggable v-model="editTasks">
        <!-- <draggable v-model="editTasks" @end="draggableEnd"> -->
          <template v-for="(task, i) in editTasks">
            <!-- <v-divider
              v-if="i !== 0"
              :key="`${i}-divider`"
            ></v-divider> -->

            <v-list-item :key="`${i}-${task.text}`">
              <v-icon>mdi-drag-vertical</v-icon>
              <div
                :class="task.done && 'grey--text' || 'primary--text'"
                class="ml-4"
                v-text="task.text"
              ></div>
            </v-list-item>
          </template>
        </draggable>
      </v-list>
      <template v-else>
        <v-list class="py-0">
          <template v-for="(task, i) in tasks">
            <v-divider
              v-if="i !== 0"
              :key="`${i}-divider`"
            ></v-divider>

            <v-list-item :key="`${i}-${task.text}`">
              <v-list-item-action>
                <v-checkbox
                  v-model="task.done"
                  :color="task.done && 'grey' || 'primary'"
                >
                  <template
                    v-if="i !== editTargetIndex"
                    v-slot:label
                  >
                    <div
                      :class="task.done && 'grey--text' || 'primary--text'"
                      class="ml-4"
                      v-text="task.text"
                    ></div>
                  </template>
                </v-checkbox>
              </v-list-item-action>

              <v-text-field
                v-if="i === editTargetIndex"
                v-model="editText"
                outlined
                dense
                hide-details
                class="ml-n5 mr-1"
              >
                <template v-slot:append>
                  <v-icon @click="saveText(task)" color="green">mdi-check-bold</v-icon>
                  <v-icon @click="endEditText" color="grey" class="ml-2">mdi-close</v-icon>
                </template>
              </v-text-field>

              <template v-else>
                <v-spacer></v-spacer>

              <!-- <v-scroll-x-transition>
                <v-icon
                  v-if="task.done"
                  color="success"
                >
                  check
                </v-icon>
              </v-scroll-x-transition> -->
                <v-btn @click="startEditText(i, task.text)" icon>
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>
              <v-btn @click="deleteTask(i)" icon class="">
                <v-icon>mdi-trash-can-outline</v-icon>
              </v-btn>
            </v-list-item>
          </template>
        </v-list>
      </template>
    </v-card>
  </v-container>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: "TodoListView",
  components: {
    draggable,
  },
  data: () => ({
    tasks: [
      {
        done: false,
        text: 'Foobar'
      },
      {
        done: false,
        text: 'Fizzbuzz'
      },
      {
        done: true,
        text: 'bubble sort'
      },
    ],
    editTasks: [],
    task: "",
    isSwap: false,
    isEdit: false,
    editText: '',
    editTargetIndex: null
  }),
  computed: {
    completedTasks() {
      return this.tasks.filter(task => task.done).length
    },
    progress() {
      return this.completedTasks / this.tasks.length * 100
    },
    remainingTasks() {
      return this.tasks.length - this.completedTasks
    }
  },
  methods: {
    create() {
      this.tasks.push({
        done: false,
        text: this.task,
      })
      this.task = ""
    },
    startSwapTasks() {
      this.editTasks = JSON.parse(JSON.stringify(this.tasks))
      this.isSwap = true
    },
    endSwapTasks() {
      // this.save()
      this.tasks = this.editTasks
      this.isSwap = false
    },
    startEditText(index, text) {
      this.isEdit = true
      this.editTargetIndex = index
      this.editText = text
    },
    endEditText() {
      this.isEdit = false
      this.editTargetIndex = null
      this.editText = ""
    },
    saveText(task) {
      task.text = this.editText
      // this.save()
      this.endEditText()
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
      this.endEditText()
    },
    draggableEnd(event) {
      console.log(event)
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>
