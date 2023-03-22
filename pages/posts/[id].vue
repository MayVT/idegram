<template>
  <div>
    <h1>Esto es el artículo numero {{ myId }}</h1>
  </div>
</template>
<script setup>
const route = useRoute()
const myId = ref(route.params.id);
</script>
