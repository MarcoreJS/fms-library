<template>
  <v-layout row wrap>
    <v-layout>
      <v-flex xs12 sm6 offset-sm3>
        <v-card>
          <v-img :src="book.image" aspect-ratio="2.75"></v-img>

          <v-card-title primary-title>
            <div>
              <h3 class="headline mb-0">{{ book.title }}</h3>
              <div>{{ book.author }}</div>
            </div>
          </v-card-title>

          <v-card-actions>
            <v-btn to="/" flat color="orange">Regresar</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      book: this.$route.params.book,
      sk: this.$route.params.sk
    };
  }
};
</script>

