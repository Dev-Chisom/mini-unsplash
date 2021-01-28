<template>
  <div>
    <header class="main__header">
      <div class="header__form-container">
        <h1 v-if="ifSearching">
          {{
            isSearching
              ? 'Searching for '
              : photos.length >= 1
              ? 'Result for '
              : 'No Result For '
          }}<span>{{ searchText }}</span>
        </h1>
        <form v-else class="header__form" @submit.prevent="HandleSearch()">
          <span class="header__icon"><i class="fa fa-search"></i></span>
          <input
            type="text"
            name="Search"
            class="header__search"
            placeholder="Search for photo"
            v-model="searchText"
          />
        </form>
      </div>
    </header>
    <section class="container">
      <div class="placeholder__container" v-if="photosLoading">
        <div
          class="placeholder-container__item placeholder"
          v-for="placeholder in 10"
          :key="placeholder"
        >
          <img class="image" />

          <div class="placeholder-container__item-info">
            <h3
              class="placeholder-container__item-name placeholder__child"
            ></h3>
            <p
              class="placeholder-container__item-location placeholder__child"
            ></p>
          </div>
        </div>
      </div>
      <div class="images">
        <div
          class="images__item"
          v-for="photo in photos"
          :key="photo.id"
          @click="zoomPhoto(true, photo.id)"
        >
          <img
            :src="photo.urls.regular"
            style="width: 100%;"
            class="images__item-image"
          />

          <h3 class="images__item-name">
            {{ photo.user.name }}
          </h3>
          <p class="images__item-location">{{ photo.user.location }}</p>
        </div>
      </div>
    </section>
    <section class="modal-section">
      <div class="modal" v-if="showModal">
        <div class="modal__close" @click="showModal = false">
          <i class="fas fa-times-circle"></i>
        </div>
        <div class="modal__body">
          <img :src="selectedPhoto.urls.regular" class="modal__image" />
          <div class="modal__info">
            <h3 class="modal__heading">
              {{ selectedPhoto.user.name }}
            </h3>
            <p class="modal__paragraph">
              {{ selectedPhoto.user.location }}
            </p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data() {
    return {
      photos: [],
      photosLoading: true,
      searchText: '',
      showModal: false,
      ifSearching: false,
      isSearching: true,
      uri: 'https://api.unsplash.com/photos/?client_id=',
      apiKey: process.env.VUE_APP_MAP_KEY,
    };
  },

  methods: {
    zoomPhoto(showModal, itemKey) {
      this.showModal = showModal;
      this.selectedPhoto = this.photos.find(function(element) {
        return element.id === itemKey;
      });
    },
    HandleSearch() {
      console.log(this.searchText);
      this.ifSearching = true;
      this.isSearching = false;
      if (this.searchText) {
        const MOVIE_API_URL = `https://api.unsplash.com/search/photos?client_id=${this.apiKey}&query=${this.searchText}`;
        console.log(MOVIE_API_URL);
        axios
          .get(MOVIE_API_URL)
          .then((response) => {
            this.photos = response.data.results;
            console.log(response.data.results);
          })

          .catch((error) => {
            console.log(error);
          });
      }
    },
  },

  created() {
    setTimeout(() => (this.photosLoading = false), 2000);
    return axios
      .get(`${this.uri}${this.apiKey}`)
      .then((response) => {
        this.photos = response.data;
        console.log(response.data);
      })

      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<style>
:root {
  --primary-grey: #dde2e9;
  --primary-white: #fff;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Roboto', sans-serif;
  background-color: #fff;
}
span {
  color: #000;
}
.main__header {
  background-color: var(--primary-grey);
  height: 25em;
  width: 100%;
}
.header__form-container {
  width: 80%;
  margin: 0 auto;
  padding-top: 10em;
}
.header__form {
  height: 3.5em;
  background-color: var(--primary-white);
  border-radius: 0.5em;
  box-shadow: 10px 10px 44px 0px rgba(0, 0, 0, 0.18);
}
.header__search {
  height: 100%;
  border: 0;
  outline: 0;
  width: 80%;
  font-size: 1.1em;
}
.header__icon {
  font-size: 1.3em;
  padding: 0 2em 0 1em;
}
.container {
  width: 80%;
  margin: -6em auto;
}
.images,
.placeholder__container {
  max-width: 980px;
  margin: -7em auto 0;
  grid-row-gap: 30px;
  /* -moz-column-gap: 40px; */
  grid-column-gap: 40px;
  column-gap: 40px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-auto-rows: 40px;
}
.images__item,
.placeholder-container__item {
  position: relative;
  width: 100%;
  height: 100%;
  color: white;
  border-radius: 0.5em;
  overflow: hidden;
  cursor: zoom-in;
  transition: all 0.5s ease;
}
.images__item:nth-child(1),
.placeholder-container__item:nth-child(1) {
  grid-row-end: span 7;
}
.images__item:nth-child(2),
.placeholder-container__item:nth-child(2) {
  grid-row-end: span 4;
}
.images__item:nth-child(3),
.placeholder-container__item:nth-child(3) {
  grid-row-end: span 5;
}
.images__item:nth-child(4),
.placeholder-container__item:nth-child(4) {
  grid-row-end: span 6;
}
.images__item:nth-child(5),
.placeholder-container__item:nth-child(5) {
  grid-row-end: span 9;
}
.images__item:nth-child(6),
.placeholder-container__item:nth-child(6) {
  grid-row-end: span 6;
}
.images__item:nth-child(7),
.placeholder-container__item:nth-child(7) {
  grid-row-end: span 4;
}
.images__item:nth-child(8),
.placeholder-container__item:nth-child(8) {
  grid-row-end: span 5;
}
.images__item:nth-child(9),
.placeholder-container__item:nth-child(9) {
  grid-row-end: span 8;
}
.images__item:nth-child(10),
.placeholder-container__item:nth-child(10) {
  grid-row-end: span 6;
}
.images__item-image,
.placeholder-container__item .image {
  object-fit: cover;
  height: 100%;
}
.images__item-name,
.placeholder-container__item-name {
  position: absolute;
  bottom: 2.5em;
  left: 1em;
  z-index: 1;
}
.images__item-location,
.placeholder-container__item-location {
  position: absolute;
  bottom: 1em;
  left: 1em;
  z-index: 1;
}
.placeholder-container__item::after {
  content: '';
  position: absolute;
  height: 100%;
  width: 100%;
  background-image: linear-gradient(
    rgba(0, 0, 0, 0.04),
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.95)
  );
  top: 0;
  left: 0;
}
.placeholder-container__item-location {
  color: var(--primary-grey);
  margin-top: 1em;
}
.placeholder {
  box-shadow: 0 4px 10px 0 rgba(33, 33, 33, 0.15);
  background-color: #f5f5f5;
}
.placeholder:after {
  content: '';
  display: block;
  position: absolute;
  left: -150px;
  top: 0;
  height: 100%;
  width: 150px;
  background-image: none;
  background: linear-gradient(
    to right,
    transparent 0%,
    #e8e8e8 50%,
    transparent 100%
  );
  animation: load 1s cubic-bezier(0.4, 0, 0.2, 1) infinite;
}
.placeholder__child {
  width: 10em;
  height: 0.5em;
  background-color: #e6e6e6;
}
@keyframes load {
  from {
    left: -150px;
  }
  to {
    left: 100%;
  }
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 99;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 50px;
}
.modal__body {
  max-width: 900px;
  background: #fff;
  width: 100%;
  height: 100%;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}
.modal__close {
  border-radius: 50%;
  padding: 0 0.4em;
  position: absolute;
  top: 10px;
  cursor: pointer;
  right: 40px;
  transition: all 0.5s;
  font-size: 4em;
  color: var(--primary-grey);
}
.modal__image {
  object-fit: cover;
  height: 83%;
  width: 100%;
}
.modal__info {
  color: rgb(53, 52, 52);
  padding: 2em 1em;
}
</style>
