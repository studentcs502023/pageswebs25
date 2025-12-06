<template>
  <div class="juego-container">
    <div class="score">
      <q-btn style="background: goldenrod; color: white" label="score" to="/score"/>
    </div>

    <!-- DIBUJO AHORCADO -->
    <div class="ahorcado">
      <div class="dibujo" v-html="dibujoHTML"></div>
    </div>

    <!-- INTENTOS RESTANTES -->
    <div class="info-juego">
      <div class="intentos">
        <span class="emoji">❤️</span>
        <span class="contador">{{ intentosRestantes }}</span>
        <span class="texto">Intentos</span>
      </div>
    </div>

    <!-- CONTADOR DE TIEMPO ACTUAL -->
    <div v-if="contadorIniciado && !juegoTerminado" class="tiempo-actual">
      <p>⏱️ Tiempo: {{ tiempo }} segundos</p>
    </div>
    
    <!-- CONTADOR DE TIEMPO CUANDO GANA -->
    <div v-if="juegoTerminado && tiempoGanado > 0" class="tiempo-ganado">
      <p>🏆 ¡Ganaste en {{ tiempoGanado }} segundos!</p>
    </div>

    <!-- PALABRA -->
    <div class="palabra-container">
      <p class="palabra">
        <span v-for="(letra, i) in letrasEncontradas" :key="i" class="letra-casilla">
          {{ letra }}
        </span>
      </p>
      <p class="mensaje">{{ mensaje }}</p>
    </div>

    <!-- TECLADO -->
    <div class="teclado">
      <button 
        v-for="(letra, i) in teclado" 
        :key="i" 
        @click="presionarLetra(letra)"
        :disabled="letrasUsadas.includes(letra) || juegoTerminado"
        :class="{
          'correcta': letrasCorrectas.includes(letra),
          'incorrecta': letrasIncorrectas.includes(letra),
          'disabled': letrasUsadas.includes(letra)
        }"
      >
        {{ letra }}
      </button>
    </div>

    <!-- BOTONES DE ACCIÓN -->
    <div class="botones-accion">
      <button @click="reiniciarJuego" class="btn-reiniciar">🔄 Reiniciar</button>
      <button @click="$router.push('/')" class="btn-inicio">🏠 Volver a Inicio</button>
    </div>

    <!-- REGLAS -->
    <div class="reglas-nivel">
      <h3>📋 Reglas del Nivel:</h3>
      <p>{{ reglasTexto }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// Array de dibujos del ahorcado
const dibujos = [
  '',
  '|\n|\n|\n|\n|',
  '+---+\n|\n|\n|\n|\n|',
  '+---+\n|   O\n|\n|\n|\n|',
  '+---+\n|   O\n|   |\n|\n|\n|',
  '+---+\n|   O\n|  /|\n|\n|\n|',
  '+---+\n|   O\n|  /|\\\n|\n|\n|',
  '+---+\n|   O\n|  /|\\\n|  /\n|\n|',
  '+---+\n|   O\n|  /|\\\n|  / \\\n|\n|'
];

// Variables reactivas
const dibujoAhorcado = ref(dibujos[0]);
const teclado = ref("abcdefghijklmnopqrstuvwxyz".split(""));

const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const letrasUsadas = ref([]);
const letrasCorrectas = ref([]);
const letrasIncorrectas = ref([]);

const intentosRestantes = ref(0);
const juegoTerminado = ref(false);
const mensaje = ref("");
const tiempo = ref(0);
const tiempoGanado = ref(0);
const contadorIniciado = ref(false);

// Variables para el contador
let startTime = null;
let timer = null;

// Computed properties
const dibujoHTML = computed(() => {
  return dibujoAhorcado.value.replace(/\n/g, "<br>");
});

const reglasTexto = computed(() => {
  const nivel = localStorage.getItem("nivelSeleccionado") || "facil";
  switch(nivel) {
    case "facil":
      return "14 intentos - Palabras cortas y comunes";
    case "medio":
      return "10 intentos - Palabras de dificultad media";
    case "dificil":
      return "9 intentos - Palabras largas y complejas";
    default:
      return "Selecciona un nivel";
  }
});

// Funciones del contador
function iniciarContador() {
  if (contadorIniciado.value) return;
  
  contadorIniciado.value = true;
  startTime = Date.now();
  timer = setInterval(actualizarTiempo, 1000);
}

function actualizarTiempo() {
  if (startTime) {
    tiempo.value = Math.floor((Date.now() - startTime) / 1000);
  }
}

function detenerContador() {
  if (timer) {
    clearInterval(timer);
    timer = null;
    contadorIniciado.value = false;
  }
}

function guardarTiempoGanado() {
  tiempoGanado.value = tiempo.value;
  detenerContador();
}

// Funciones del juego
function obtenerPalabrasDelStorage() {
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");
  if (!palabrasCategoria) return null;
  
  try {
    const palabras = JSON.parse(palabrasCategoria);
    return Array.isArray(palabras) && palabras.length > 0 ? palabras : null;
  } catch (error) {
    console.error("Error al parsear palabras:", error);
    return null;
  }
}

function obtenerNivel() {
  return localStorage.getItem("nivelSeleccionado") || "facil";
}

function configurarIntentosPorNivel(nivel) {
  switch(nivel) {
    case "facil":
      return 14;
    case "medio":
      return 10;
    case "dificil":
      return 9;
    default:
      return 10;
  }
}

function iniciarJuego() {
  // Detener contador si está corriendo
  detenerContador();
  
  // Resetear todas las variables
  palabraAleatoria.value = "";
  letrasEncontradas.value = [];
  letrasUsadas.value = [];
  letrasCorrectas.value = [];
  letrasIncorrectas.value = [];
  dibujoAhorcado.value = dibujos[0];
  juegoTerminado.value = false;
  mensaje.value = "";
  tiempo.value = 0;
  tiempoGanado.value = 0;
  contadorIniciado.value = false;
  startTime = null;
  
  // Obtener nivel y palabras
  const nivel = obtenerNivel();
  const palabras = obtenerPalabrasDelStorage();
  
  if (!palabras) {
    mensaje.value = "❌ No hay palabras disponibles. Regresa al inicio y selecciona una categoría.";
    return;
  }
  
  // Seleccionar palabra aleatoria
  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
  
  // Inicializar letras encontradas con guiones bajos
  letrasEncontradas.value = Array(palabraAleatoria.value.length).fill("_");
  
  // Configurar intentos según nivel
  intentosRestantes.value = configurarIntentosPorNivel(nivel);
  
  console.log("Juego iniciado. Palabra:", palabraAleatoria.value);
  console.log("Nivel:", nivel, "Intentos:", intentosRestantes.value);
}

function presionarLetra(letra) {
  // Verificar condiciones para presionar letra
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) {
    return;
  }
  
  // Iniciar contador en la primera letra
  if (letrasUsadas.value.length === 0) {
    iniciarContador();
  }
  
  // Agregar letra a las usadas
  letrasUsadas.value.push(letra);
  
  const letraMinuscula = letra.toLowerCase();
  const palabraMinuscula = palabraAleatoria.value.toLowerCase();
  
  // Verificar si la letra está en la palabra
  if (!palabraMinuscula.includes(letraMinuscula)) {
    // Letra incorrecta
    manejarLetraIncorrecta(letra);
  } else {
    // Letra correcta
    manejarLetraCorrecta(letra);
  }
}

function manejarLetraIncorrecta(letra) {
  letrasIncorrectas.value.push(letra);
  intentosRestantes.value--;
  
  // Actualizar dibujo del ahorcado
  const indiceDibujo = Math.min(letrasIncorrectas.value.length, dibujos.length - 1);
  dibujoAhorcado.value = dibujos[indiceDibujo];
  
  mensaje.value = `❌ Letra "${letra}" incorrecta. Te quedan ${intentosRestantes.value} intentos`;
  
  // Verificar si perdió
  if (intentosRestantes.value <= 0) {
    juegoTerminado.value = true;
    mensaje.value = `💀 ¡Perdiste! La palabra era: "${palabraAleatoria.value}"`;
    detenerContador();
  }
}

function manejarLetraCorrecta(letra) {
  letrasCorrectas.value.push(letra);
  
  // Revelar todas las ocurrencias de la letra
  const letraMinuscula = letra.toLowerCase();
  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i].toLowerCase() === letraMinuscula) {
      // Mantener el caso original (mayúscula/minúscula)
      letrasEncontradas.value[i] = palabraAleatoria.value[i];
    }
  }
  
  mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;
  
  // Verificar si ganó
  if (!letrasEncontradas.value.includes("_")) {
    juegoTerminado.value = true;
    mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;
    guardarTiempoGanado();
  }
}

function reiniciarJuego() {
  iniciarJuego();
}

// Hooks del ciclo de vida
onMounted(() => {
  iniciarJuego();
});

onUnmounted(() => {
  detenerContador();
});
</script>




<style scoped>
.juego-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.score {
  position: absolute;
  top: 20px;
  right: 20px;
  margin-top: 100px;
}

.ahorcado .dibujo {
  font-family: 'Courier New', monospace;
  font-size: 18px;
  line-height: 1.5;
  margin: 20px 0;
  min-height: 120px;
}

.info-juego {
  margin: 20px 0;
}

.intentos {
  display: inline-flex;
  align-items: center;
  background: #ff6b6b;
  color: white;
  padding: 8px 16px;
  border-radius: 20px;
  font-weight: bold;
}

.emoji {
  font-size: 24px;
  margin-right: 8px;
}

.contador {
  font-size: 24px;
  margin: 0 8px;
}

.texto {
  font-size: 16px;
}

.tiempo-actual {
  background: #e3f2fd;
  border: 2px solid #2196f3;
  border-radius: 10px;
  padding: 10px;
  margin: 10px auto;
  display: inline-block;
  color: #0d47a1;
  font-weight: bold;
}

.tiempo-ganado {
  background: #e8f5e9;
  border: 3px solid #4caf50;
  border-radius: 10px;
  padding: 15px;
  margin: 15px auto;
  color: #2e7d32;
  font-weight: bold;
  font-size: 18px;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.palabra-container {
  margin: 30px 0;
}

.palabra {
  font-size: 32px;
  letter-spacing: 10px;
  margin: 20px 0;
  font-weight: bold;
}

.letra-casilla {
  display: inline-block;
  min-width: 40px;
  height: 50px;
  border-bottom: 3px solid #333;
  margin: 0 5px;
  text-align: center;
  line-height: 50px;
}

.mensaje {
  font-size: 18px;
  font-weight: bold;
  min-height: 30px;
  margin: 15px 0;
  color: #333;
}

.teclado {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 8px;
  max-width: 600px;
  margin: 20px auto;
  padding: 15px;
  background: #f5f5f5;
  border-radius: 10px;
}

.teclado button {
  width: 45px;
  height: 45px;
  font-size: 18px;
  font-weight: bold;
  border: 2px solid #ccc;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
}

.teclado button:hover:not(:disabled) {
  background: #e0e0e0;
  transform: translateY(-2px);
}

.teclado button.correcta {
  background-color: #4caf50;
  color: white;
  border-color: #388e3c;
}

.teclado button.incorrecta {
  background-color: #f44336;
  color: white;
  border-color: #d32f2f;
}

.teclado button.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.botones-accion {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 30px 0;
}

.btn-reiniciar, .btn-inicio {
  padding: 12px 24px;
  font-size: 16px;
  font-weight: bold;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
}

.btn-reiniciar {
  background: #ff9800;
  color: white;
}

.btn-reiniciar:hover {
  background: #f57c00;
  transform: translateY(-2px);
}

.btn-inicio {
  background: #2196f3;
  color: white;
}

.btn-inicio:hover {
  background: #1976d2;
  transform: translateY(-2px);
}

.reglas-nivel {
  background: #fff8e1;
  border: 2px solid #ffd54f;
  border-radius: 10px;
  padding: 15px;
  margin: 20px auto;
  max-width: 500px;
}

.reglas-nivel h3 {
  margin: 0 0 10px 0;
  color: #ff8f00;
}

.reglas-nivel p {
  margin: 0;
  color: #5d4037;
  font-weight: bold;
}
</style>