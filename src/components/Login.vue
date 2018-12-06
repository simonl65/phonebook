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
            <v-toolbar-title>Login</v-toolbar-title>
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field
                v-model="email"
                prepend-icon="person"
                name="email"
                label="Email"
                type="text"
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
              @click="logIn"
            >Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
    data(){
      return {
        email:null,
        password:null
      }
    },

    methods: {
      logIn(){
        axios.post(
          '/login',
          {
            email   : this.email,
            password: this.password
          }
        )
        .then((response) => {
          console.log('response:', response);

          if( undefined == response.data )
          {
            throw new Error('Sorry - Login failed');
          }

          let accessToken = response.data.auth.access_token;
          localStorage.setItem('token', accessToken);
          localStorage.setItem('user', response.data.user.name);

          window.isSignedIn = true;
        })
        .catch(error => {
          alert(error.message);
        });
      }
    }
}
</script>
