<template>
  <div class="container .is-fullhd container-film">
    <p class="title-list">
      {{
        showForm ? `Form ${type === "create" ? "Tambah" : "Ubah"} Film` : "List Film 2023"
      }}
    </p>
    <div v-if="!showForm">
      <div style="display: flex; justify-content: end; padding-right: 40px">
        <button class="button is-success is-medium" @click="goToForm('create')">
          Tambah Film Baru
        </button>
      </div>
      <div v-if="!isLoading && films.length > 0" class="wrapper-list-film">
        <Card
          v-for="(key, index) in films"
          :id="key.id"
          :key="index"
          :img="key.film_img"
          :title="key.title"
          :genre="key.genre"
          :duration="key.duration"
          :director="key.director"
          :writer="key.writer"
          :language="key.language"
          :urlTrailer="key.trailer"
          :synopsis="key.synopsis"
          @goToUpdate="goToForm('edit', key.id)"
        />
      </div>
      <div v-else-if="isLoading" class="wrapper-list-film">
        <ShimmerCard v-for="(key, index) in 8" :key="index" />
      </div>
      <div style="display: flex; justify-content: center; margin-top: 15%; flex-direction:column;" v-else>
        <p style="font-size: 20px; font-weight: bolder; text-align:center;">
          Tidak Ada Film, Silahkan refresh kembali...
        </p>
      </div>
    </div>
    <div v-else>
      <Form :form="form" @goBack="getList" @onSubmit="getList" :type="type" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./components/Card.vue";
import ShimmerCard from "./components/ShimmerCard.vue";
import Form from "./components/Form.vue";

export default {
  name: "App",
  components: { Card, ShimmerCard, Form },
  mounted() {
    this.getListFilm();
  },
  data() {
    return {
      isLoading: false,
      showForm: false,
      films: [],
      type: "",
      form: {
        id: 0,
        title: "",
        genre: "",
        duration: 0,
        director: "",
        writer: "",
        language: "",
        poster: "",
        trailer: "",
        synopsis: "",
      },
    };
  },
  methods: {
    async getListFilm() {
      this.isLoading = true;
      await axios
        .get(`api/film`)
        .then((res) => {
          this.films = res.data;
        })
        .catch((err) => {
          alert(err);
        })
        .finally(() => {
          setTimeout(() => {
            this.isLoading = false;
          }, 1000);
        });
    },
    getList() {
      this.showForm = false;
      this.getListFilm();
    },
    async getDetailFilm(id) {
      await axios
        .get(`api/film/${id}`)
        .then((res) => {
          this.form = {
            id: res.data.id,
            title: res.data.title,
            genre: res.data.genre,
            duration: res.data.duration,
            director: res.data.director,
            writer: res.data.writer,
            language: res.data.language,
            poster: res.data.film_img,
            trailer: res.data.trailer,
            synopsis: res.data.synopsis,
          };
        })
        .catch((err) => {
          alert(err);
        });
    },
    goToForm(type, id) {
      this.showForm = true;
      this.type = type;
      if (type === "edit") {
        this.getDetailFilm(id);
      }
    },
  },
};
</script>

<style>
.container-film {
  padding-top: 50px;
}
.wrapper-list-film {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  row-gap: 5px;
  justify-items: center;
  padding-top: 20px;
}

.title-list {
  font-size: 30px;
  font-weight: bold;
  font-family: "Poppins", sans-serif;
  margin-bottom: 25px;
  text-align: center;
}
</style>
