<template>
  <v-app>
    <v-navigation-drawer app v-model="drawer" clipped>
      <v-container>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="title grey--text text--darken-2">
              Navigation lists
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-divider></v-divider>
        <v-list dense nav>
          <v-list-item
            v-for="nav_list in nav_lists"
            :key="nav_list.name"
            :to="{ name: nav_list.link }"
          >
            <v-list-item-icon>
              <v-icon>{{ nav_list.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title>{{ nav_list.name }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-container>
    </v-navigation-drawer>
    <v-app-bar color="primary" dark app clipped-left>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Vuetify</v-toolbar-title>
      <v-spacer />
      <v-toolbar-items>
        <v-btn text>Button</v-btn>
      </v-toolbar-items>
    </v-app-bar>

    <v-content>
      <router-view />
    </v-content>
    <v-footer color="primary" dark app>
      Vuetify
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      drawer: false,
      nav_lists: [
        { name: "Home", icon: "mdi-home", link: "Home" },
        { name: "About", icon: "mdi-circle", link: "About" },
        {
          name: "Todo List",
          icon: "mdi-checkbox-marked-outline",
          link: "TodoList"
        }
      ]
    };
  },
  methods: {
    clickHamburgerMenu() {
      this.drawer = !this.drawer;
    }
  }
};
</script>
