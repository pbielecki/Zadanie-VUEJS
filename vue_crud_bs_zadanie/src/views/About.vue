<template>
  <div>
    <h1>Dodawanie pliku</h1>
    <div class="container jumbotron">
    <input  type="file" @change="onFileSelected">
    </div>
  </div>
</template>

<script>
export default {
  name: 'about'
}
</script>

<style>

</style>
