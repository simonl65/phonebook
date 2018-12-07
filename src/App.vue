<template>
  <v-app>
    <v-toolbar>
      <v-toolbar-title>
        <v-btn
          :to="{name: 'Home' }"
          flat>{{ title }}</v-btn>
      </v-toolbar-title>
      <v-spacer/>
      <v-toolbar-items class="hidden-sm-and-down">
        <v-btn
          v-for="(menu, index) in menus"
          :key="index"
          :to="{name:menu.route}"
          flat
        >
          {{ menu.name }}
        </v-btn>

      </v-toolbar-items>
    </v-toolbar>
    <router-view/>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      title    : "Phonebook",
      menus    : [],
      authMenu : [{ name: "Logout", route: "Logout" }],
      guestMenu: [
        { name: "Signup", route: "Signup" },
        { name: "Login", route: "Login" }
      ],
    };
  },

  created() {
    this.menus = this.guestMenu;

    Bus.$on('loggedIn', () => {
      this.onLoggedIn();
    });

    Bus.$on('logout', () => {
      this.onLogout();
    });

  },

  methods: {
    onLoggedIn() {
      this.menus = this.authMenu;
    },

    onLogout() {
      this.menus = this.guestMenu;
    }
  },
};
</script>
