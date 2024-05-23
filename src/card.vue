<template>
  <div class="card">
    <img class=" description card-top" :data-src="props.item.image">
    <div class="card-bottom">
      <p class="description name"> name: '{{ props.item.name }}'</p>
      <p class="description species">{{ props.item.species }}:{{ props.item.status }}</p>
      <p class="description location">last known location: '{{ props.item.location.name }}'</p>
    </div>
  </div>
</template>

<script setup>
  import {defineProps} from 'vue'
  const props = defineProps([
    'item'
  ])
</script>

<style>

</style>