<template>
  <div>

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
              <v-flex
                xs12
                sm6
                md4>
                <v-text-field
                  v-model="editedItem.name"
                  label="Dessert name"/>
              </v-flex>
              <v-flex
                xs12
                sm6
                md4>
                <v-text-field
                  v-model="editedItem.calories"
                  label="Calories"/>
              </v-flex>
              <v-flex
                xs12
                sm6
                md4>
                <v-text-field
                  v-model="editedItem.fat"
                  label="Fat (g)"/>
              </v-flex>
              <v-flex
                xs12
                sm6
                md4>
                <v-text-field
                  v-model="editedItem.carbs"
                  label="Carbs (g)"/>
              </v-flex>
              <v-flex
                xs12
                sm6
                md4>
                <v-text-field
                  v-model="editedItem.protein"
                  label="Protein (g)"/>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer/>
          <v-btn
            color="blue darken-1"
            flat
            @click="close">Cancel</v-btn>
          <v-btn
            color="blue darken-1"
            flat
            @click="save">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-data-table
      :headers="headers"
      :items="contacts"
      class="elevation-1" >
      <template
        slot="items"
        slot-scope="props">
        <td>{{ props.item.id }}</td>
        <td class="text-xs-right">{{ props.item.fullName }}</td>
        <td class="text-xs-right">{{ props.item.tel }}</td>
        <td class="justify-center layout px-0">
          <v-icon
            small
            class="mr-2"
            @click="editItem(props.item)">
            edit
          </v-icon>
          <v-icon
            small
            @click="deleteItem(props.item)">
            delete
          </v-icon>
        </td>
      </template>
      <template slot="no-data">
        <v-btn
          color="primary"
          @click="initialize">Reset</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        { text: 'ID', align: 'left', value: 'id' },
        { text: 'Name', value: 'fullName' },
        { text: 'Phone', value: 'tel' },
        { text: 'Actions', value: 'name', sortable: false }
      ],
      contacts: [],
      editedIndex: -1,
      editedItem: {
        id      : 0,
        fullName: '',
        tel     : '',
      },
      defaultItem: {
        id      : 0,
        fullName: '',
        tel     : '',
      }
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      }
    },

    watch: {
      dialog (val) {
        val || this.close()
      }
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize() {
        this.fetchContacts;
      },

      fetchContacts() {
        axios.get('/contact')
        .then( response => {
          console.log(response);
          this.contacts = response.data.data;
        })
      },

      editItem (item) {
        this.editedIndex = this.contacts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.contacts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.contacts.splice(index, 1)
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.contacts[this.editedIndex], this.editedItem)
        } else {
          this.contacts.push(this.editedItem)
        }
        this.close()
      }
    }
  }
</script>
