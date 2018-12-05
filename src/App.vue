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
          :to="{name: menu.route}"
          flat
        >
          {{ menu.name }}
        </v-btn>

        <v-btn
          color="success"
          @click="getInfo"
        >
          get userinfo
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
      title: "Phonebook",
      menus: [
        { name: "Signup", route: "Signup" },
        { name: "Login", route: "Login" }
      ]
    };
  },

  mounted() {
    if( token ) {
      let authMenu = [{ name: "Logout", route: "Logout" }];
      this.menus = authMenu;
    }
  },

  methods: {
    getInfo() {
      axios.get('http://passport/api/user')
      .then( response => {
        console.log(response)
      })
    }
  },
};
</script>

<style scoped>
</style>
