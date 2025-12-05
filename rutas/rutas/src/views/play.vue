<template>
  <div class="juego-container">

    <!-- INTENTOS RESTANTES Y PUNTUACIÓN -->
    <div class="info-juego">
      <div class="intentos">
        <span class="emoji">❤️</span>
        <span class="contador">{{ intentosRestantes }}</span>
        <span class="texto">Intentos</span>
      </div>
      
      <div class="puntuacion">
        <span class="emoji">⭐</span>
        <span class="contador">0</span>
        <span class="texto">Puntos</span>
      </div>
    </div>

    <!-- PALABRA EN FORMATO "_ _ _ _" CON ESTILO AHORCADO -->
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
      <button @click="reiniciarJuego" class="btn-reiniciar">
        🔄 Reiniciar
      </button>
      <button @click="$router.push('/inicio')" class="btn-inicio">
        🏠 Volver a Inicio
      </button>
    </div>

    <!-- REGLAS DEL NIVEL -->
    <div class="reglas-nivel">
      <h3>📋 Reglas del Nivel:</h3>
      <p>{{ reglasTexto }}</p>
    </div>

  </div>
</template>

<script setup>
import { onMounted, ref, computed } from 'vue';

// TECLADO
const teclado = ref([
  "a","b","c","d","e","f","g","h","i","j","k","l","m",
  "n","o","p","q","r","s","t","u","v","w","x","y","z"
]);

// PALABRA Y LETRAS
const palabraAleatoria = ref("");
const letrasEncontradas = ref([]);
const letrasUsadas = ref([]);
const letrasCorrectas = ref([]);
const letrasIncorrectas = ref([]);

// ESTADO DEL JUEGO
const intentosRestantes = ref(0);
const juegoTerminado = ref(false);
const mensaje = ref("");

// ON MOUNTED
onMounted(() => {
  iniciarJuego();
});

function iniciarJuego() {
  const nivel = localStorage.getItem("nivelSeleccionado");
  const categoria = localStorage.getItem("categoriaSeleccionada");
  const palabrasCategoria = localStorage.getItem("palabrasCategoria");

  console.log("Nivel recibido:", nivel);
  console.log("Categoría recibida:", categoria);
  console.log("Palabras recibidas:", palabrasCategoria);

  // Reiniciar estado
  letrasUsadas.value = [];
  letrasCorrectas.value = [];
  letrasIncorrectas.value = [];
  juegoTerminado.value = false;
  mensaje.value = "";

  if (!palabrasCategoria) {
    console.warn("No hay palabras guardadas en localStorage");
    return;
  }

  let palabras;
  try {
    palabras = JSON.parse(palabrasCategoria);
  } catch (e) {
    console.error("Error al convertir JSON de palabrasCategoria:", e);
    return;
  }

  if (!Array.isArray(palabras) || palabras.length === 0) {
    console.warn("palabrasCategoria no contiene un array válido");
    return;
  }

  // Elegir palabra aleatoria
  palabraAleatoria.value = palabras[Math.floor(Math.random() * palabras.length)];
  console.log("Palabra elegida:", palabraAleatoria.value);

  // Crear array de "_" con misma longitud
  letrasEncontradas.value = palabraAleatoria.value.split("").map(() => "_");

  // Definir intentos según nivel
  if (nivel === "facil") intentosRestantes.value = 14;
  else if (nivel === "medio") intentosRestantes.value = 10;
  else if (nivel === "experto") intentosRestantes.value = 9;

  console.log("Intentos disponibles:", intentosRestantes.value);
}

// REGLAS DEL NIVEL
const reglasTexto = computed(() => {
  const nivel = localStorage.getItem("nivelSeleccionado");
  switch(nivel) {
    case "facil": return "14 intentos - Palabras cortas y comunes";
    case "medio": return "10 intentos - Palabras de dificultad media";
    case "experto": return "9 intentos - Palabras largas y complejas";
    default: return "Selecciona un nivel";
  }
});

// FUNCION PARA PRESIONAR LETRA
function presionarLetra(letra) {
  if (juegoTerminado.value || letrasUsadas.value.includes(letra)) return;

  // Marcar letra como usada
  letrasUsadas.value.push(letra);

  // Letra incorrecta
  if (!palabraAleatoria.value.toLowerCase().includes(letra.toLowerCase())) {
    letrasIncorrectas.value.push(letra);
    intentosRestantes.value--;
    mensaje.value = `❌ Letra "${letra}" incorrecta. Te quedan ${intentosRestantes.value} intentos`;

    if (intentosRestantes.value <= 0) {
      juegoTerminado.value = true;
      mensaje.value = `💀 ¡Perdiste! La palabra era: "${palabraAleatoria.value}"`;
    }
    return;
  }

  // Letra correcta
  letrasCorrectas.value.push(letra);
  mensaje.value = `✅ ¡Correcto! Letra "${letra}" encontrada`;

  // Actualizar letras encontradas
  for (let i = 0; i < palabraAleatoria.value.length; i++) {
    if (palabraAleatoria.value[i].toLowerCase() === letra.toLowerCase()) {
      letrasEncontradas.value[i] = letra;
    }
  }

  // Verificar si ganó
  if (!letrasEncontradas.value.includes("_")) {
    juegoTerminado.value = true;
    mensaje.value = `🎉 ¡Ganaste! ¡Felicidades!`;
  }
}

// REINICIAR JUEGO
function reiniciarJuego() {
  iniciarJuego();
}
</script>

<style scoped>
.juego-container {
  min-height: 100vh;
  padding: 30px 20px;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  color: white;
  font-family: 'Segoe UI', 'Arial', sans-serif;
}

/* INFO DEL JUEGO */
.info-juego {
  display: flex;
  gap: 30px;
  margin-bottom: 40px;
  background: rgba(255, 255, 255, 0.1);
  padding: 15px 30px;
  border-radius: 20px;
  backdrop-filter: blur(10px);
}

.intentos, .puntuacion {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.emoji {
  font-size: 2.5rem;
  filter: drop-shadow(0 3px 5px rgba(0,0,0,0.3));
}

.contador {
  font-size: 2.2rem;
  font-weight: 800;
  color: #FFD166;
}

.texto {
  font-size: 0.9rem;
  color: #a0a0d0;
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* PALABRA */
.palabra-container {
  text-align: center;
  margin: 40px 0;
}

.palabra {
  display: flex;
  gap: 15px;
  justify-content: center;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.letra-casilla {
  width: 60px;
  height: 70px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  font-weight: 800;
  background: rgba(255, 255, 255, 0.15);
  border-bottom: 4px solid #FF6B6B;
  border-radius: 10px 10px 0 0;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  text-transform: uppercase;
}

.mensaje {
  font-size: 1.4rem;
  min-height: 40px;
  margin-top: 20px;
  padding: 10px 20px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(5px);
}

/* TECLADO */
.teclado {
  display: grid;
  grid-template-columns: repeat(9, 1fr);
  gap: 12px;
  max-width: 600px;
  margin: 30px auto;
  padding: 25px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 20px;
  backdrop-filter: blur(10px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.teclado button {
  width: 50px;
  height: 50px;
  font-size: 1.4rem;
  font-weight: 700;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  background: linear-gradient(145deg, #2D3436, #1E272E);
  color: white;
  text-transform: uppercase;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.teclado button:hover:not(:disabled) {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
}

.teclado button:active:not(:disabled) {
  transform: translateY(-2px);
}

.teclado button.correcta {
  background: linear-gradient(145deg, #00b894, #00a085);
  color: white;
}

.teclado button.incorrecta {
  background: linear-gradient(145deg, #d63031, #c0392b);
  color: white;
}

.teclado button.disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none !important;
}

/* BOTONES DE ACCIÓN */
.botones-accion {
  display: flex;
  gap: 20px;
  margin: 30px 0;
}

.botones-accion button {
  padding: 15px 30px;
  font-size: 1.1rem;
  font-weight: 600;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 10px;
}

.btn-reiniciar {
  background: linear-gradient(145deg, #0984e3, #6c5ce7);
  color: white;
}

.btn-inicio {
  background: linear-gradient(145deg, #e17055, #d63031);
  color: white;
}

.botones-accion button:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
}

/* REGLAS DEL NIVEL */
.reglas-nivel {
  background: rgba(255, 255, 255, 0.1);
  padding: 20px 30px;
  border-radius: 15px;
  margin-top: 20px;
  text-align: center;
  backdrop-filter: blur(5px);
  border-left: 4px solid #4ECDC4;
}

.reglas-nivel h3 {
  margin: 0 0 10px 0;
  color: #FFD166;
}

.reglas-nivel p {
  margin: 0;
  color: #a0a0d0;
  font-size: 1rem;
}

/* RESPONSIVE */
@media (max-width: 768px) {
  .teclado {
    grid-template-columns: repeat(7, 1fr);
    gap: 10px;
    padding: 20px;
  }
  
  .teclado button {
    width: 45px;
    height: 45px;
    font-size: 1.2rem;
  }
  
  .letra-casilla {
    width: 45px;
    height: 55px;
    font-size: 2rem;
  }
  
  .info-juego {
    flex-direction: column;
    gap: 20px;
  }
  
  .botones-accion {
    flex-direction: column;
    width: 100%;
    max-width: 300px;
  }
}

@media (max-width: 480px) {
  .teclado {
    grid-template-columns: repeat(6, 1fr);
  }
  
  .teclado button {
    width: 40px;
    height: 40px;
    font-size: 1.1rem;
  }
  
  .letra-casilla {
    width: 35px;
    height: 45px;
    font-size: 1.7rem;
    gap: 8px;
  }
  
  .info-juego {
    padding: 10px 20px;
  }
}
</style>