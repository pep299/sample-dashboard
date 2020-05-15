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
      <v-btn icon class="mx-4">
        <v-icon>mdi-swap-vertical</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
    </v-row>

    <v-divider class="mb-4"></v-divider>

    <v-card v-if="tasks.length > 0">
      <v-slide-y-transition>
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
                  <template v-slot:label>
                    <div
                      :class="task.done && 'grey--text' || 'primary--text'"
                      class="ml-4"
                      v-text="task.text"
                    ></div>
                  </template>
                </v-checkbox>
              </v-list-item-action>

              <v-spacer></v-spacer>

              <!-- <v-scroll-x-transition>
                <v-icon
                  v-if="task.done"
                  color="success"
                >
                  check
                </v-icon>
              </v-scroll-x-transition> -->
              <v-btn icon>
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn icon class="">
                <v-icon>mdi-trash-can-outline</v-icon>
              </v-btn>
            </v-list-item>
          </template>
        </v-list>
      </v-slide-y-transition>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "TodoListView",
  data: () => ({
    tasks: [
      {
        done: false,
        text: 'Foobar',
      },
      {
        done: false,
        text: 'Fizzbuzz',
      },
    ],
    task: null
  }),
  computed: {
    completedTasks () {
      return this.tasks.filter(task => task.done).length
    },
    progress () {
      return this.completedTasks / this.tasks.length * 100
    },
    remainingTasks () {
      return this.tasks.length - this.completedTasks
    }
  },
  methods: {
    create () {
      this.tasks.push({
        done: false,
        text: this.task,
      })
      this.task = ""
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  max-width: 500px;
}
</style>
