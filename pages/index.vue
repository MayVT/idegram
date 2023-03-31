<template>
  <div>
    <Loading :show="verLoading" />

    <button type="button" class="btn btn-success mb-3" @click="showModalNewPhoto = true">Subir foto</button>
    <h1 class="mt-3 mb-3">Imágenes subidas</h1>
    <Card v-for="photo in photos" :url="photo.url" :likes="photo.likes" :id="photo.id" />
    <div class="modal" tabindex="-1" :class="{'d-block': showModalNewPhoto}">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Subir imagen</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="showModalNewPhoto = false"></button>
          </div>
          <div class="modal-body">
            <input type="file" ref="inputFile">
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="showModalNewPhoto = false">Cancelar</button>
            <button @click="subirFoto" type="button" class="btn btn-primary">Subir</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
// Variables
import { createClient } from '@supabase/supabase-js'

// Create a single supabase client for interacting with your database
const supabase = createClient('https://zvzfjgymikbmbfetmuql.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Inp2emZqZ3ltaWtibWJmZXRtdXFsIiwicm9sZSI6ImFub24iLCJpYXQiOjE2Nzk1NjA3NjcsImV4cCI6MTk5NTEzNjc2N30.YZRc9OX-8nAkwWMpzZV3eyI7axb9QDgCi6UsnvX3pWI')
const inputFile = ref();
const photos = ref();
const showModalNewPhoto = ref(false);
const verLoading = ref(false);


// Funciones
async function obtenerFotos() {
  const { data, error } = await supabase
      .from('photos' )
      .select()
      .order('id', { ascending: false})
  photos.value = data;
 }

 async function vigilarCambiosEnTabla() {
   supabase
       .channel('any')
       .on('postgres_changes', { event: 'UPDATE', schema: 'public', table: 'photos' }, payload => {
         obtenerFotos();
       })
       .subscribe()
 }

async function subirFoto() {
  photos.value = [];
  // Ver Loading
  verLoading.value = true;
  // Cerramos modal
  showModalNewPhoto.value = false;
  // Generamos la ruta de la imagen
  const url = `public/${crypto.randomUUID()}.jpg`;
  // Subimos la imagen al bucket
  const {data, errorUpload} = await supabase
      .storage
      .from("photos")
      .upload(url, inputFile.value.files[0], {
        cacheControl: "3600",
        upsert: false
      })
  // Guardamos la ruta en la base de datos
  const { errorTable } = await supabase
      .from('photos')
      .insert({ url: url, id_user: "anonimo" })

  // Refrescamos las fotos que se muestran
  await obtenerFotos();
  // Quitar Loading
  verLoading.value = false;



}

// Cuabdo se cargue la página, llamará a todo el contenido de onMounted
onMounted(async () => {
  // Ver loading
  verLoading.value = true;
  await obtenerFotos();
  // Quitar loading
  verLoading.value = false;
  // Vigilamos los cambios de los likes
  vigilarCambiosEnTabla();
})

</script>