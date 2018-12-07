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
        <h3>You are logged out.</h3>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      email: null,
      password: null
    }
  },

  created() {
    Bus.$emit('logout');
    localStorage.removeItem('token');
    localStorage.removeItem('user');
    delete window.token;
  }
}
</script>
