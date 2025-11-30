<template>
    <div class="color">

        <div class="center">
<div class="tittle" style="display: flex; align-items: center; gap: 20px;">
<div style="display: flex; align-items: center; gap: 20px;">
    <pre style="margin: 0; ">
  +---+
  |   |
  O   |
 /|\  |
 / \  |
      |
=========
    </pre>

    <h1 style="margin: 0; ">AHORCADO</h1>

  
</div>

  


</div>



<div class="img">
    <img src="/public/iamage.png" alt="imagen">
</div>
<q-btn
  label="Play Now"
  to="/categorias"
  glossy
  color="purple"
/>


</div>


    </div>
</template>

<style>
.color{
    background: aliceblue; /* Gris muy oscuro con tono azulado */
    width: 100%;
    height: 87vh;
    overflow-y: hidden;
}

.center{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
     

}
.tittle{
    display: flex;
    flex-direction: column;
}

.img{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.img img{
    width: 500px;
    height: auto;
}

</style>


<script setup>

</script>


<!-- <template>
  <q-layout view="lHh Lpr lFf" class="bg-white">


    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-2"
    >

          <q-btn color="primary" label="Inicio" to="/inicio"/><br>
       <q-btn color="primary" label="Categorias" to="/categorias"/><br>
       <q-btn color="primary" label="Nivel" to="/nivel"/><br>
        <q-btn color="primary" label="Animales" to="/animales"/><br>
       <q-btn color="primary" label="Score" to="/score"/><br>
      
    </q-drawer>

    <q-page-container>
      <router-view />
      <h1>holacocmo fsad</h1>

    </q-page-container>
  </q-layout>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'MyLayout',

  setup () {
    const leftDrawerOpen = ref(false)

    function toggleLeftDrawer () {
      leftDrawerOpen.value = !leftDrawerOpen.value
    }

    return {
      leftDrawerOpen,
      toggleLeftDrawer
    }
  }
}
</script> -->