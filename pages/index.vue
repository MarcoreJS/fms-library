<template>
  <v-layout column justify-center align-center>
    <v-flex xs12>
      <v-card class="pa-4">
        <label>
          <v-icon>attach_file</v-icon>Book Image:
        </label>
        <input type="file" :multiple="false" ref="fileInput" @change="onFileChange" />
      </v-card>
    </v-flex>

    <v-flex xs12 text-xs-right>
      <v-btn class="mx-0 font-weight-light" color="success" @click="newBook()">Guardar</v-btn>
    </v-flex>
  </v-layout>
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
      config: {
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Content-Type": "application/json"
        }
			},
      url: "https://dniv5uxo5i.execute-api.us-west-1.amazonaws.com/dev/books",
      book: {
        title: "Prueba",
        author: "Prueba",
        publisher: "Prueba",
        category: "Prueba",
        onStock: 12,
        price: 12,
        image: ""
      }
    };
  },
  methods: {
    newBook() {
      this.$axios.post(this.url, this.book, this.config.headers).then(res => {
        console.log(res)
      }).catch(e => console.log(e.message));
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
    }
  }
};
</script>
