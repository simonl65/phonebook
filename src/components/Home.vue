<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout
        column
        align-center>
        <img
          src="@/assets/logo.png"
          alt="Vuetify.js"
          class="mb-5">

        <!-- <div v-for="(contact, index) in contacts" :key="index">
          <h3>{{ conact.mobile }}</h3>
        </div> -->
        <PhonebookDataTable />

      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import PhonebookDataTable from './PhonebookDataTable';

export default {

  components: {
    PhonebookDataTable
  },

  data() {
    return {
      contacts: [],
    }
  },

  mounted() {
    this.fetchContacts();
  },

  methods: {
    fetchContacts() {
      axios.get('/contact')
      .then( response => {
        console.log(response);
        this.contacts = response.data.data;
      })
    }
  },
}
</script>
