<template>
  <div class="container">
    <form @submit.prevent="get('new')" class="search-block">
      <input v-model="searchText" type="text" />
      <select v-model="searchStatus">
        <option selected value="Dead">Dead</option>
        <option value="Alive">Alive</option>
      </select>
      <button type="submit">find</button>
    </form>
    <div class="results">
      <card v-for="one of result?.results" :key="one.id" :item="one"></card>
    </div>
    <div class="pagination">
      <button
        :disabled="result?.info?.prev == null"
        class="prev"
        @click="get('prev')"
      >
        prev
      </button>
      <div class="pages">{{ page }} / {{ result?.info?.pages }}</div>
      <button
        :disabled="result?.info?.next == null"
        class="next"
        @click="get('next')"
      >
        next
      </button>
    </div>
  </div>
</template>

<script setup>
const observer = new IntersectionObserver(
  (entries, observer) => {
    entries.forEach((one) => {
      one.isIntersecting == true
        ? (one.target.src = one.target.dataset.src)
        : (one.target.src = "");
    });
  },
  {
    root: document.querySelector(".results"),
    rootMargin: "10px",
  }
);
setTimeout(() => {
  document
    .querySelectorAll(".card-top")
    .forEach((one) => observer.observe(one));
}, 200);
import { defineComponent, ref } from "vue";
import card from "./card.vue";
defineComponent({
  card,
});
const searchText = ref("rick");
const page = ref(1);
const searchStatus = ref("Dead");
const result = ref({});
function get(what) {
  // document.querySelectorAll(".card-top").forEach((one) => observer.unobserve(one));
  setTimeout(() => {
    document
      .querySelectorAll(".card-top")
      .forEach((one) => observer.observe(one));
  }, 100);
  if (what == "prev") {
    fetch(result.value.info.prev)
      .then((res) => (res = res.json()))
      .then((res) => (result.value = res));
    page.value--;
  } else if (what == "next") {
    fetch(result.value.info.next)
      .then((res) => (res = res.json()))
      .then((res) => (result.value = res));
    page.value++;
  } else {
    fetch(
      `https://rickandmortyapi.com/api/character?${
        searchText.value != "" ? "&name=" + searchText.value : ""
      }${searchStatus.value != "" ? "&status=" + searchStatus.value : ""}`
    )
      .then((res) => (res = res.json()))
      .then((res) => (result.value = res));
    page.value = 1;
  }
}
fetch(
  `https://rickandmortyapi.com/api/character?name=${searchText.value}&status=${searchStatus.value}`
)
  .then((res) => (res = res.json()))
  .then((res) => (result.value = res));
</script> 

