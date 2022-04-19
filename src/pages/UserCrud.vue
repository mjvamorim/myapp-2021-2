<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
           <h1> UserCrud </h1>
           <v-data-table
              :headers="headers"
              :items="users"
              :search="search"
              sort-by="id"
              class="elavation-1">
           ##Formulario
          <template v-slot:top>
            <v-toolbar flat>
              <v-toolbar-title>User Crud</v-toolbar-title>
              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="700px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    dark
                    class="mb-2"
                    v-bind="attrs"
                    v-on="on"
                  >
                    Novo Item
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="headline">Users</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.id"
                            label="Id"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="8">
                          <v-text-field
                            v-model="editedItem.name"
                            label="Nome"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="12" sm="6" md="4">
                          <v-text-field
                            v-model="editedItem.location"
                            label="Localização"
                          ></v-text-field>
                        </v-col>
                       
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close">
                      Cancel
                    </v-btn>
                    <v-btn color="blue darken-1" text @click="save">
                      Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          ##Fim Form
           </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios"

export default {
  name: "UserCrud",
  data: () => ({
    search:"",
    dialog: false,
    headers: [
      { text: "Id", value: "id"},
      { text: "Nome", value: "name"},
      { text: "País", value: "location"},
      { text: "Actions", value: "actions", sortable: false}
    ],
    users: [],
    editedItem: {id: 0, name: "", location: ""},
    defaultItem: {id: 0, name: "", location: "Brasil"},
    editedIndex: -1
  }),
  methods: {
    inicializa() {
      axios.get("http://localhost:3000/users")
        .then((response) => {
          this.users = response.data
        })
        .catch((error)=> console.log(error))
    },
    close() {
      this.dialog = false;
      setTimeout( () => {
        this.editedItem = Object.assign( {}, this.defaultItem);
        this.editedIndex = -1;
      }, 300)
    },
    save() {
      if (this.editedIndex == -1) { //incluindo
         axios
          .post("http://localhost:3000/users", this.editedItem)
          .then((response) => {
            console.log(response);
            this.users.push(this.editedItem);
            this.close();
          })

      }
    }
  },
  created() {
    this.inicializa()
  }
};
</script>