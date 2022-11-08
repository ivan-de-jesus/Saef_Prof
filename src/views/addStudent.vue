<template>
  <v-data-table
    :headers="headers"
    :items="desserts"
    sort-by="albumNumerus"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Alumnos registrados</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2 pl-6"
              v-bind="attrs"
              v-on="on"
            >
              Nuevo alumno
            </v-btn>
            <v-btn
              color="warning"
              dark
              class="mb-2 pr-6"
              v-bind="attrs"
              v-on="on"
            >
              Asignar ejercicios
            </v-btn>
          </template>

          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.name"
                      label="Nombre del alumno"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.albumNumerus"
                      label="No.Lista"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.gradus"
                      label="Grado"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.group"
                      label="Grupo"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.age"
                      label="Edad"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      v-model="editedItem.pass"
                      label="Contraseña"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close"> Cancelar </v-btn>
              <v-btn color="blue darken-1" text @click="save"> Guardar </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5"
              >Are you sure you want to delete this item?</v-card-title
            >
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete"
                >Cancel</v-btn
              >
              <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                >OK</v-btn
              >
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)"> mdi-pencil </v-icon>
      <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize"> Reset </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Nombre del alumno",
        align: "start",
        sortable: false,
        value: "name",
      },
      { text: "No. Lista", value: "albumNumerus" },
      { text: "Grado", value: "gradus" },
      { text: "Grupo", value: "group" },
      { text: "Edad", value: "age" },
      { text: "Contraseña", value: "pass" },
      { text: "Accion", value: "actions", sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      name: "",
      albumNumerus: 1,
      gradus: 0,
      group: 0,
      age: 0,
    },
    defaultItem: {
      name: "",
      albumNumerus: 0,
      gradus: 0,
      group: 0,
      age: 0,
    },
    idDelete:null,
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Agregar alumno" : "Editar alumno";
    },
  },
  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },
  created() {
    this.initialize();
    this.getStudents();
  },
  methods: {
    initialize() {
      this.desserts = [];
    },
    async getStudents() {
      const res = await this.axios.get("http://localhost:3000/students");
      console.log(res);
      this.desserts = res.data;
    },
    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },
    deleteItem(item) {
      console.log("Item seleccionado",item._id);
      this.idDelete = item._id;
      this.editedIndex = this.desserts.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },
    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1);
      this.closeDelete();
      let url = "http://localhost:3000/delete/" + this.idDelete;
      const res =  this.axios.get(url);
      
    },
    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },
    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem);
      } else {
        this.desserts.push(this.editedItem);
        this.axios
          .post("http://localhost:3000/create",this.editedItem,
            {
              params: {
                name: this.editedItem.name,
                albumNumerus: this.editedItem.albumNumerus,
                gradus: this.editedItem.gradus,
                group: this.editedItem.group,
                age: this.editedItem.age,
                pass: this.editedItem.pass
              },
            }
          )
          .then((res) => {
            console.log(res);
          })
          .catch((err) => {
            console.log(err);
          });
      }
      this.close();
    },
  },
};
</script>
