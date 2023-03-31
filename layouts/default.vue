<template>
  <div>
    <Head>
      <Title>Idegram</Title>
      <Meta name="description" content="App para compartir fotos" />
      <Link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous" />
    </Head>
    <div class="custom-container">
      <header>
        <h1 class="text-center">Idegram</h1>
      </header>
      <main>
        <slot />
      </main>
    </div>
    <footer>2023</footer>
  </div>
</template>

<style scope>
  .custom-container {
    padding: 1rem;
    margin: 0 auto;
    max-width: 50rem;
  }

</style>