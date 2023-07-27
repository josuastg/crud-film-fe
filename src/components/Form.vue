<template>
  <div>
    <div class="content-form columns">
      <div class="column">
        <div class="field">
          <label class="label">Judul Film *</label>
          <div class="control">
            <input
              v-model="form.title"
              class="input is-success"
              type="text"
              placeholder="Masukkan Judul Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Genre Film *</label>
          <div class="control">
            <input
              v-model="form.genre"
              class="input is-success"
              type="text"
              placeholder="Masukkan Genre Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Durasi Film (Menit) *</label>
          <div class="control">
            <input
              v-model="form.duration"
              class="input is-success"
              type="number"
              placeholder="Masukkan Durasi Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Sutradara Film *</label>
          <div class="control">
            <input
              v-model="form.director"
              class="input is-success"
              type="text"
              placeholder="Masukkan Sutradara Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Penulis Film *</label>
          <div class="control">
            <input
              v-model="form.writer"
              class="input is-success"
              type="text"
              placeholder="Masukkan Penulis Film"
            />
          </div>
        </div>
      </div>
      <div class="column">
        <div class="field">
          <label class="label">Bahasa *</label>
          <div class="control">
            <input
              v-model="form.language"
              class="input is-success"
              type="text"
              placeholder="Masukkan Bahasa"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Poster Film *</label>
          <div class="control">
            <input
              v-model="form.poster"
              class="input is-success"
              type="text"
              placeholder="Masukkan Link Poster Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Trailer Film *</label>
          <div class="control">
            <input
              v-model="form.trailer"
              class="input is-success"
              type="text"
              placeholder="Masukkan Link Trailer Film"
            />
          </div>
        </div>
        <div class="field">
          <label class="label">Synopsis *</label>
          <div class="control">
            <textarea
              v-model="form.synopsis"
              class="textarea is-success"
              type="text"
              placeholder="Masukkan Synopsis Film"
            />
          </div>
        </div>
      </div>
    </div>
    <div style="display: flex; justify-content: center; margin-top: 25px">
      <div class="buttons">
        <button class="button is-info" @click="goBack">Kembali</button>
        <button v-if="type === 'edit'" @click="onDelete" class="button is-danger">
          Hapus
        </button>
        <button
          class="button is-success"
          @click="type === 'edit' ? onUpdate() : onSubmit()"
          :disabled="!isFormValid"
        >
          {{ type === "edit" ? "Ubah" : "Simpan" }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Form",
  event: ["goBack", "onSubmit"],
  props: {
    type: String,
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
  },
  mounted() {
    this.form = {
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
    };
  },
  data() {
    return {};
  },
  computed: {
    isFormValid() {
      return (
        this.form.title &&
        this.form.genre &&
        this.form.duration &&
        this.form.director &&
        this.form.writer &&
        this.form.language &&
        this.form.poster &&
        this.form.trailer &&
        this.form.synopsis
      );
    },
  },
  methods: {
    goBack() {
      this.$emit("goBack");
    },
    async onDelete() {
      await axios
        .delete(`api/film/delete/${this.form.id}`)
        .then(() => {
          this.form = {
            title: "",
            genre: "",
            duration: 0,
            director: "",
            writer: "",
            language: "",
            poster: "",
            trailer: "",
            synopsis: "",
          };
          alert("Berhasil menghapus film");
          this.$emit("onSubmit");
        })
        .catch((err) => {
          alert(err.response?.data?.message);
        });
    },
    async onSubmit() {
      await axios
        .post(`api/film/create`, {
          title: this.form.title,
          genre: this.form.genre,
          duration: +this.form.duration,
          synopsis: this.form.synopsis,
          director: this.form.director,
          writer: this.form.writer,
          film_img: this.form.poster,
          trailer: this.form.trailer,
          language: this.form.language,
        })
        .then(() => {
          this.form = {
            title: "",
            genre: "",
            duration: 0,
            director: "",
            writer: "",
            language: "",
            poster: "",
            trailer: "",
            synopsis: "",
          };
          alert("Berhasil menambahkan film baru");
          this.$emit("onSubmit");
        })
        .catch((err) => {
          alert(err.response?.data?.message);
        });
    },
    async onUpdate() {
      await axios
        .put(`api/film/update/${this.form.id}`, {
          title: this.form.title,
          genre: this.form.genre,
          duration: +this.form.duration,
          synopsis: this.form.synopsis,
          director: this.form.director,
          writer: this.form.writer,
          film_img: this.form.poster,
          trailer: this.form.trailer,
          language: this.form.language,
        })
        .then(() => {
          this.form = {
            title: "",
            genre: "",
            duration: 0,
            director: "",
            writer: "",
            language: "",
            poster: "",
            trailer: "",
            synopsis: "",
          };
          alert("Berhasil mengubah data film");
          this.$emit("onSubmit");
        })
        .catch((err) => {
          alert(err.response?.data?.message);
        });
    },
  },
};
</script>

<style scoped>
.content-form {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin-top: 3%;
  margin-left: 10%;
  margin-right: 10%;
  margin-bottom: 3%;
}

label {
  font-family: "Poppins", sans-serif;
}
</style>
