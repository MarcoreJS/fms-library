<template>
  <v-container grid-list-xl fluid>
    <v-layout row wrap>
      <v-flex md6>
        <v-text-field v-model="book.title" label="Titulo"></v-text-field>
      </v-flex>
      <v-flex md6>
        <v-text-field v-model="book.author" label="Author"></v-text-field>
      </v-flex>
      <v-flex md4>
        <v-text-field v-model="book.publisher" label="Editorial"></v-text-field>
      </v-flex>
      <v-flex md4>
        <v-text-field v-model="book.category" label="Categoria"></v-text-field>
      </v-flex>
      <v-flex md2>
        <v-text-field v-model="book.price" label="Precio"></v-text-field>
      </v-flex>
      <v-flex md2>
        <v-text-field v-model="book.onStock" label="Inventario" mask="####"></v-text-field>
      </v-flex>
      <v-flex xs12>
        <label>
          <v-icon>attach_file</v-icon>Foto del Libro:
        </label>
        <input type="file" :multiple="false" ref="fileInput" @change="onFileChange" />
      </v-flex>

      <v-flex xs12 text-xs-right>
        <v-btn class="mx-0 font-weight-light" color="success" @click="newBook()">Guardar</v-btn>
      </v-flex>

      <v-flex sm12>
        <v-data-table
          class="elevation-1"
          :rows-per-page-items="[10, 25, 100, 200]"
          :headers="headers"
          :items="books"
        >
          <template v-slot:items="props">
            <td>{{ props.item.title }}</td>
            <td>{{ props.item.author }}</td>
            <td>{{ props.item.category }}</td>
            <td>{{ props.item.price }}</td>
            <td>{{ props.item.onStock }}</td>
            <td>
              <v-btn
                :to="{ name: 'libro-sk', params: { sk: props.item.sk, book: props.item }}"
                outline
              >Ver</v-btn>
            </td>
            <td>
              <v-btn @click="deleteBook(props.item.sk)" color="red" outline>Eliminar</v-btn>
            </td>
          </template>
        </v-data-table>

        <v-dialog v-model="dialog" width="300">
          <v-card>
            <v-card-text>
              <h3 v-text="text"></h3>
              <v-progress-linear
                :active="show"
                :indeterminate="query"
                color="#BF9B30"
                class="mb-0"
                max="100"
              ></v-progress-linear>
            </v-card-text>
          </v-card>
        </v-dialog>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data() {
    return {
      show: true,
      text: "Cargando...",
      dialog: false,
      query: true,
      headers: [
        { text: "Titulo", value: "title" },
        { text: "Autor", value: "author" },
        { text: "Categoria", value: "category" },
        { text: "Precio", value: "price" },
        { text: "En Inventario", value: "onStock" },
        { text: "Ver" },
        { text: "Eliminar" }
      ],
      config: {
        headers: {
          "x-api-key": "J2ZdrUISFb8p1677T8EoZ8RAaoAX4nJG6q31OJp3",
          "Content-Type": "application/json"
        }
      },
      url: "https://dniv5uxo5i.execute-api.us-west-1.amazonaws.com/dev/books",
      book: {
        title: "",
        author: "",
        publisher: "",
        category: "",
        onStock: 0,
        price: 0,
        image: ""
      },
      books: []
    };
  },
  methods: {
    newBook() {
      this.dialog = true;
      this.$axios
        .post(this.url, this.book, this.config)
        .then(res => {
          console.log(res);
          let aux_res = JSON.parse(res.data.body);
          alert("Registrado: " + aux_res.Item.title);
          this.getBooks();
          this.dialog = false;
          this.query = false;
        })
        .catch(e => console.log(e.message));
    },
    onFileChange($event) {
      const files = $event.target.files || $event.dataTransfer.files;
      const form = this.getFormData(files);
      this.getBase64([...files][0]);
    },
    getBase64(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => (this.book.image = reader.result);
        reader.onerror = error => reject(error);
      });
    },
    getFormData(files) {
      const data = new FormData();
      return data;
    },
    getBooks() {
      this.$axios
        .get(
          "https://qo62e2m977.execute-api.us-west-1.amazonaws.com/dev",
          this.config
        )
        .then(res => {
          this.books = res.data.body.Items;
        });
    },
    deleteBook(sk) {
      this.$axios
        .delete(
          "https://qo62e2m977.execute-api.us-west-1.amazonaws.com/dev/books/" + sk,
          this.config
        )
        .then(res => {
          this.getBooks();
          alert("Eliminado")
        }).catch(e => alert(e.message))
    }
  },
  created() {
    this.getBooks();
  }
};
</script>
