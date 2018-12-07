<template>
  <v-container
    fluid
    fill-height
  >
    <v-layout
      align-center
      justify-center
    >
      <v-flex
        xs12
        sm8
        md4
      >
        <v-card class="elevation-12">
          <v-toolbar
            dark
            color="primary"
          >
            <v-toolbar-title>Signup form</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field
                v-model="name"
                prepend-icon="person"
                name="name"
                label="Name"
                type="text"
              />
              <v-text-field
                v-model="email"
                prepend-icon="email"
                name="email"
                label="Email"
                type="email"
              />
              <v-text-field
                id="password"
                v-model="password"
                prepend-icon="lock"
                name="password"
                label="Password"
                type="password"
              />
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer/>
            <v-btn
              color="primary"
              type="submit"
              @click="registerUser"
            >Signup Now</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      email   : null,
      name    : null,
      password: null
    }
  },

  methods: {
    registerUser() {
      /**
       * Attempt to get the access token:
       */
      axios.post(
        '/register',
        {
          name    : this.name,
          email   : this.email,
          password: this.password
        }
      )
      .then((response) => {
        let accessToken = response.data.auth.access_token;
        localStorage.setItem('token', accessToken);
        localStorage.setItem('user', response.data.user.name);

        window.isSignedIn = true;

        Bus.$emit('loggedIn');

        this.$router.push('/');
      })
      .catch(error => {
        console.error(error)
      });
    }
  }
}
</script>
