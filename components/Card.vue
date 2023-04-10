<template>
  <article class="mb-5">
    <!-- imagen -->
    <div class="ratio ratio-1x1">
      <img @click="addLike" :src="urlPublic" alt="" class="img object-fit-cover border rounded">
    </div>
    <!-- Controles -->
    <div class="d-flex justify-content-end">
      <button @click="addLike" class="btn btn-outline-primary p-3 mt-3 text-center" aria-current="page">❤ {{ likes }}</button>
    </div>
  </article>
</template>
<script setup>
import { createClient } from '@supabase/supabase-js'
// Variables
// Create a single supabase client for interacting with your database
const supabase = createClient('https://zvzfjgymikbmbfetmuql.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Inp2emZqZ3ltaWtibWJmZXRtdXFsIiwicm9sZSI6ImFub24iLCJpYXQiOjE2Nzk1NjA3NjcsImV4cCI6MTk5NTEzNjc2N30.YZRc9OX-8nAkwWMpzZV3eyI7axb9QDgCi6UsnvX3pWI')
// Variables
const urlPublic = ref();
const props = defineProps({
  id: Number,
  url: String,
  likes: Number
})


 onMounted(async() => {
   // Obtener la URL publica de la imagen
  const { data }  = await supabase
      .storage
      .from('photos')
      .getPublicUrl(props.url);
  urlPublic.value = data.publicUrl;
})

async function addLike () {
  const { error } = await supabase
      .from('photos')
      .update({ likes: props.likes + 1 })
      .eq('id', props.id)
}

</script>

<style scope>
  .img {
    width: 100%;
  }
</style>
