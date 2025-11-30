<template>
  <div>

    <!-- PALABRA EN FORMATO "_ _ _ _" -->
    <p class="palabra">
      <span v-for="(l, i) in letrasEncontradas" :key="i">
        {{ l }}
      </span>
    </p>

    <!-- TECLADO -->
    <div class="teclado">
      <button v-for="(letra, i) in teclado" :key="i" @click="presionarLetra(letra)">
        {{ letra }}
      </button>
    </div>

  </div>
</template>


<script setup>
import { onMounted, ref } from 'vue';

const teclado = ref([
  "a","b","c","d","e","f","g","h","i","j","k","l","m",
  "n","o","p","q","r","s","t","u","v","w","x","y","z"
]);
const respuesta = ref("");
const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const keyfind = ref("");

onMounted(() => {
  const nivel = localStorage.getItem("nivelSeleccionado");
  const categoria = localStorage.getItem("categoriaSeleccionada");
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  console.log("Nivel recibido:", nivel);
  console.log("Categoria recibida:", categoria);
  console.log("Palabras recibidas:", palabrasCategoria);

  if (!palabrasCategoria) {
    console.warn("No hay palabras guardadas");
    return;
  }

  const palabras = JSON.parse(palabrasCategoria);

  // Elegir palabra aleatoria:
  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
console.log("esta es la palabra", palabraAleatoria.value)
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");
});
function presionarLetra(letra) {
  keyfind.value = letra;

  // Revisar si esa letra está en la palabra
  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i] === letra) {
      letrasEncontradas.value[i] = letra; // reemplaza el guion
    }
  }
}
</script>
