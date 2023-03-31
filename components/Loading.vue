<template>
  <div>
    <div class="d-flex fixed-top vh-100 vw-100 bg-light opacity-75 justify-content-center align-items-center" :class="{'d-none': !show}">
      <div class="spinner-border text-success" role="status">
        <span class="visually-hidden"></span>
      </div>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  show: Boolean
})

</script>