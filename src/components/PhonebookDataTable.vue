<template>
  <div class="table-wrapper">
    <v-dialog
      v-model="dialog"
      max-width="500px">
      <v-btn
        slot="activator"
        color="primary"
        dark
        class="mb-2">New Contact</v-btn>
      <v-card>
        <v-card-title>
          <span class="headline">{{ formTitle }}</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <!-- <v-flex xs12 sm1 md1>
                <v-text-field v-model="editedItem.id" label="ID"></v-text-field>
              </v-flex>-->
              <v-flex
                xs12
                sm6
                md6>
                <v-text-field
                  v-model="editedItem.fullName"
                  label="Full Name"/>
              </v-flex>
              <v-flex
                xs12
                sm6
                md6>
                <v-text-field
                  v-model="editedItem.tel"
                  label="Phone"/>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer/>
          <v-btn
            color="blue darken-1"
            flat
            @click.native="close">Cancel</v-btn>
          <v-btn
            color="blue darken-1"
            flat
            @click.native="save">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-text-field
      v-model="search"
      append-icon="search"
      label="Search"
      single-line
      hide-details/>

    <v-data-table
      :headers="headers"
      :items="contacts"
      :search="search"
      :pagination.sync="pagination"
      hide-actions
      class="elevation-1"
    >
      <template
        slot="items"
        slot-scope="props">
        <td class="text-xs-left">{{ props.item.id }}</td>
        <td class="text-xs-left">{{ props.item.fullName }}</td>
        <td class="text-xs-left">{{ props.item.tel }}</td>
        <td class="justify-center layout px-0">
          <v-btn
            icon
            class="mx-0"
            @click="editItem(props.item)">
            <v-icon color="teal">edit</v-icon>
          </v-btn>
          <v-btn
            icon
            class="mx-0"
            @click="deleteItem(props.item)">
            <v-icon color="pink">delete</v-icon>
          </v-btn>
        </td>
      </template>
      <template slot="no-data">
        <v-btn
          color="primary"
          @click="initialize">Reset</v-btn>
      </template>
    </v-data-table>

    <div class="text-xs-center pt-2">
      <v-pagination
        v-model="pagination.page"
        :length="pages"/>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    search: "",
    pagination: { rowsPerPage: 5 },
    dialog: false,
    headers: [
      { text: "ID", value: "id" },
      { text: "Full Name", value: "fullName" },
      { text: "Phone", value: "tel" },
      { text: "Actions", value: "id", sortable: false }
    ],
    contacts: [],
    editedIndex: -1,
    editedItem: {
      id: 0,
      fullName: "",
      tel: ""
    },
    defaultItem: {
      id: 0,
      fullName: "",
      tel: ""
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Contact" : "Edit Contact";
    },

    pages() {
      if (
        this.pagination.rowsPerPage == null ||
        this.pagination.totalItems == null
      ) {
        return 0;
      }

      return Math.ceil(
        this.pagination.totalItems / this.pagination.rowsPerPage
      );
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
  },

  methods: {
    fetchContacts() {
      axios.get("/contact").then(response => {
        console.log("GET RESPONSE:", response);
        this.contacts = response.data.data;
      });
    },

    initialize() {
      this.fetchContacts();
    },

    editItem(item) {
      this.editedIndex = this.contacts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      console.log("DELETE CONTACT:", item);

      const index = this.contacts.indexOf(item);

      confirm("Are you sure you want to delete this item?") &&
        this.contacts.splice(index, 1);

      axios.delete("/contact/" + item.id).then(response => {
        console.log("DELETE RESPONSE:", response);
      });
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        console.log("EDIT CONTACT:", this.editedItem);

        axios
          .put("/contact/" + this.editedItem.id, {
            name: this.editedItem.fullName,
            phone: this.editedItem.tel
          })
          .then(response => {
            console.log("PUT RESPONSE:", response);
          });

        Object.assign(this.contacts[this.editedIndex], this.editedItem);
      } else {
        console.log("CREATE CONTACT:", this.editedItem);

        axios
          .post("/contact", {
            name: this.editedItem.fullName,
            phone: this.editedItem.tel
          })
          .then(response => {
            console.log("POST RESPONSE:", response);
          });

        this.contacts.push(this.editedItem);
      }

      this.close();
    }
  }
};
</script>

<style>
.table-wrapper {
  min-width: 75%;
}
</style>
