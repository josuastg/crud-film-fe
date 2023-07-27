<template>
  <div>
    <div class="wrapper-card" @click="showModal = true">
      <img :src="img" alt="image" class="img-card" />
      <p class="title-card">{{ title }}</p>
    </div>
    <transition name="fade" appear>
      <div
        class="modal-detail-card-overlay"
        v-if="showModal"
        @click="showModal = false"
      ></div>
    </transition>
    <transition name="pop" appear>
      <div class="modal-detail-card" role="dialog" v-if="showModal">
        <p class="title-modal-card">{{ title }}</p>
        <div>
          <div class="content-detail-card">
            <div class="content-image">
              <img :src="img" alt="image" class="img-card" />
              <button
                class="button is-link"
                style="margin-top: 5px"
                @click="showTrailer(urlTrailer)"
              >
                <p style="margin: 0px">Tonton Cuplikan</p>
              </button>
              <button
                @click="goToUpdate(id)"
                class="button is-warning"
                style="margin-top: 5px; padding-right: 50px; padding-left: 50px"
              >
                <p style="margin: 0px">Ubah</p>
              </button>
            </div>
            <div class="content-text">
              <div class="content-title">
                <p>Genre</p>
                <p>Sutradara</p>
                <p>Penulis</p>
                <p>Bahasa</p>
                <p>Durasi</p>
              </div>
              <div class="content-subtitle">
                <p>: {{ genre }}</p>
                <p>: {{ director }}</p>
                <p>: {{ writer }}</p>
                <p>: {{ language }}</p>
                <p>: {{ duration }} Menit</p>
              </div>
            </div>
          </div>
          <div class="section-synopsis">
            <p class="title-card">Sinopsis</p>
            <p style="text-align: justify">
              {{ synopsis }}
            </p>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    id: Number,
    img: String,
    title: String,
    urlTrailer: String,
    genre: String,
    director: String,
    writer: String,
    language: String,
    duration: Number,
    synopsis: String,
  },
  event: "goToUpdate",
  data() {
    return {
      showModal: false,
    };
  },
  methods: {
    showTrailer(url) {
      window.open(url);
    },
    goToUpdate() {
      this.$emit("goToUpdate");
    },
  },
};
</script>

<style scoped>
.section-synopsis {
  padding-left: 20px;
}
p {
  font-family: "Poppins", sans-serif;
  font-size: 16px;
  margin-bottom: 30px;
  text-align: left;
}
.content-text {
  display: flex;
  flex-direction: row;
  width: 100%;
  margin-left: 20px;
}
.content-title {
  display: flex;
  flex-direction: column;
  width: 20%;
}
.content-subtitle {
  display: flex;
  flex-direction: column;
  width: 70%;
}
.wrapper-card {
  display: flex;
  flex-direction: column;
  width: 16em;
  height: 20em;
  margin-bottom: 180px;
}
.img-card {
  height: auto;
  object-fit: contain;
  border-radius: 5px;
}

.content-image {
  width: 14em;
  height: 18em;
}

.content-detail-card {
  width: 100%;
  display: flex;
  flex-direction: row;
  padding: 18px 0px 20px 20px;
}
.wrapper-card:hover {
  box-shadow: 0 0 11px rgba(33, 33, 33, 0.2);
  cursor: pointer;
}

.title-card {
  text-align: center;
  font-family: "Poppins", sans-serif;
  font-size: 19px;
  font-weight: bold;
  margin-top: 0.5em;
}

.title-modal-card {
  text-align: center;
  font-family: "Poppins", sans-serif;
  font-size: 22px;
  font-weight: bold;
  margin-top: 0.5em;
}

.modal-detail-card {
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  text-align: center;
  width: 50em;
  height: 40em;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 5px 5px rgba(0, 0, 0, 0.2);
  background: #fff;
  z-index: 999;
  transform: none;
  overflow-y: auto;
}
.modal-detail-card h1 {
  margin: 0 0 1rem;
}

.modal-detail-card-overlay {
  content: "";
  position: absolute;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 998;
  background: #d3d3d3;
  opacity: 0.8;
  cursor: pointer;
}

/* ---------------------------------- */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s linear;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.pop-enter-active,
.pop-leave-active {
  transition: transform 0.4s cubic-bezier(0.5, 0, 0.5, 1), opacity 0.4s linear;
}

.pop-enter,
.pop-leave-to {
  opacity: 0;
  transform: scale(0.3) translateY(-50%);
}

::-webkit-scrollbar-track {
  /* -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3); */
  border-radius: 10px;
  /* background-color: #f5f5f5; */
}

::-webkit-scrollbar {
  width: 12px;
}

::-webkit-scrollbar-thumb {
  border-radius: 10px;
  /* background-color: #555; */
}
</style>
