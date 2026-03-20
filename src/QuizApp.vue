<template>
  <div class="page-wrapper">
    <header class="site-header">
      <img :src="avatarUrl" alt="Avatar" class="avatar" />
      <p class="subtitle">Escuela Sabática: Lección 12</p>
      <h1>Vivir en comunión con los demás</h1>
    </header>

    <main class="quiz-app">
      <!-- Stats -->
      <div class="stats">
        Aciertos: <span class="score-value">{{ score }}</span> / {{ questions.length }}
      </div>

      <!-- Quiz activo -->
      <template v-if="!quizFinished">
        <div class="question-container">
          <div class="question-text">
            {{ currentQuestionIndex + 1 }}. {{ currentQuestion.q }}
          </div>

          <div class="options-grid">
            <button
              v-for="(opt, i) in currentQuestion.options"
              :key="i"
              class="option-btn"
              :class="{
                correct: answered && i === currentQuestion.correct,
                wrong: answered && i === selectedIndex && i !== currentQuestion.correct,
              }"
              :disabled="answered"
              @click="checkAnswer(i)"
            >
              {{ opt }}
            </button>
          </div>
        </div>

        <!-- Feedback -->
        <div v-if="answered" class="feedback-box">
          <strong>{{ feedbackTitle }}</strong>
          <p>{{ currentQuestion.explanation }}</p>
        </div>

        <button
          v-if="answered && !isLastQuestion"
          class="nav-btn"
          @click="nextQuestion"
        >
          Siguiente pregunta
        </button>
      </template>

      <!-- Resultados -->
      <template v-else>
        <div class="results">
          <h2>¡Quiz Finalizado!</h2>
          <p>Tu puntuación final es de <strong>{{ score }}</strong> sobre <strong>{{ questions.length }}</strong>.</p>
        </div>
        <div class="retry-wrapper">
          <button class="nav-btn" @click="retryQuiz">Reintentar Quiz</button>
        </div>
      </template>
    </main>

    <footer>
      <p>Escuela Sabática &copy; 2026</p>
    </footer>
  </div>
</template>

<script>
import avatarUrl from '/assets/avatar.png';

export default {
  name: 'QuizApp',

  data() {
    return {
      avatarUrl,

      questions: [
        {
          q: "¿Cuál es el medio de 'conectividad e interactividad' elegido por el Cielo para apoyarnos en nuestras luchas?",
          options: ['La meditación trascendental', 'La oración', 'El estudio intelectual solo', 'El aislamiento social'],
          correct: 1,
          explanation:
            'La oración es el medio de conectividad e interactividad elegido por el Cielo. Dios concede, en respuesta a la oración de fe, lo que no daría si no se lo pidiésemos.',
        },
        {
          q: '¿A qué edad indica Elena G. de White que los niños deben entender que deben llevar su parte de las cargas de la vida?',
          options: ['A los 12 años', 'A los 18 años', 'Desde los 6 años en adelante', 'Cuando terminan la escuela'],
          correct: 2,
          explanation:
            'Todos, desde el niño de seis años en adelante, tienen que comprender que se requiere que lleven su parte de las cargas de la vida para contribuir al orden de la familia.',
        },
        {
          q: 'Según Colosenses 4:6, ¿cómo debe ser nuestra palabra para saber responder a cada uno?',
          options: ['Directa y tajante', 'Siempre agradable y sazonada con sal', 'Misteriosa y profunda', 'Escueta y silenciosa'],
          correct: 1,
          explanation:
            "Nuestras palabras deben ser impulsadas por la gracia y 'sazonadas con sal', es decir, apropiadas y agradables para quienes nos dirigimos.",
        },
        {
          q: '¿Qué factor tiene un impacto más sorprendente en la permanencia de los hijos en la iglesia cuando son adultos?',
          options: ['La asistencia de los amigos', 'La asistencia constante del padre', 'La asistencia exclusiva de la madre', 'La decoración de la iglesia'],
          correct: 1,
          explanation:
            'La asistencia constante a la iglesia por parte del padre incide profundamente en la permanencia espiritual de los hijos en su adultez.',
        },
        {
          q: '¿Cuál es la finalidad principal del conocimiento revelado en la Palabra de Dios?',
          options: ['Satisfacer la sed intelectual', 'Ganar debates teológicos', 'Aplicarlo a nuestra vida práctica', 'Aprender idiomas antiguos'],
          correct: 2,
          explanation:
            'La finalidad del conocimiento bíblico no es solo intelectual, sino que lo apliquemos a nuestra vida cotidiana para mejorar la convivencia.',
        },
        {
          q: '¿Qué metáfora utiliza el texto para describir un hogar donde se cultiva el amor y la verdadera cortesía?',
          options: ['Un cuartel de disciplina', 'Un pequeño Cielo en la Tierra', 'Un centro de debate', 'Una empresa de lucro'],
          correct: 1,
          explanation:
            'El hogar debiera ser un pequeño Cielo en la Tierra, donde se cultiven los afectos y la cortesía mutua en lugar de reprimirlos.',
        },
        {
          q: 'En el matrimonio, ¿qué principio ilustra cómo debe ser el amor del marido hacia su esposa?',
          options: ['El amor de los padres', 'El amor manifestado por Cristo a la iglesia', 'La conveniencia social', 'El afecto romántico pasajero'],
          correct: 1,
          explanation:
            'El amor de Cristo al entregarse por la iglesia ilustra la fidelidad y el sacrificio que el marido debe tener hacia su esposa.',
        },
        {
          q: '¿Qué instrucción da Pablo a los padres en Colosenses 3:21 para proteger la salud emocional de los hijos?',
          options: ['Que les den todo lo que pidan', 'Que no los irriten para que no se desalienten', 'Que sean indiferentes', 'Que solo usen castigos severos'],
          correct: 1,
          explanation:
            "La instrucción es equilibrada: 'Padres, no irriten a sus hijos, para que no se desalienten', reconociendo el impacto psicológico de la disciplina irritante.",
        },
        {
          q: '¿Cuál es la primera obligación de un trabajador/esclavo en el contexto de la fe cristiana?',
          options: ['Complacer al amo en todo', 'Su obligación para con el Señor', 'Buscar solo su beneficio', 'Evitar el trabajo'],
          correct: 1,
          explanation:
            'A diferencia del sistema romano, en la iglesia la primera obligación de todo trabajador es para con el Señor, trabajando de corazón.',
        },
        {
          q: "¿Qué descuido constituye, según el texto, la 'suma de la amargura y la tristeza de la vida'?",
          options: ['La falta de dinero', 'El descuido de las pequeñas cortesías y actos de amor', 'No viajar por el mundo', 'Tener opiniones diferentes'],
          correct: 1,
          explanation:
            'El descuido de las cosas pequeñas —gestos de gratitud y tierna bondad— constituye la suma de la amargura en la convivencia diaria.',
        },
      ],

      currentQuestionIndex: 0,
      score: 0,
      answered: false,
      selectedIndex: null,
      quizFinished: false,
    }
  },

  computed: {
    currentQuestion() {
      return this.questions[this.currentQuestionIndex]
    },
    isLastQuestion() {
      return this.currentQuestionIndex === this.questions.length - 1
    },
    feedbackTitle() {
      return this.selectedIndex === this.currentQuestion.correct ? '¡Correcto!' : 'Incorrecto'
    },
  },

  methods: {
    checkAnswer(index) {
      if (this.answered) return
      this.answered = true
      this.selectedIndex = index

      if (index === this.currentQuestion.correct) {
        this.score++
      }

      if (this.isLastQuestion) {
        // Pequeño delay para mostrar el feedback antes de los resultados
        setTimeout(() => {
          this.quizFinished = true
        }, 1800)
      }
    },

    nextQuestion() {
      this.currentQuestionIndex++
      this.answered = false
      this.selectedIndex = null
    },

    retryQuiz() {
      this.currentQuestionIndex = 0
      this.score = 0
      this.answered = false
      this.selectedIndex = null
      this.quizFinished = false
    },
  },
}
</script>

<style scoped>
/* ── Variables ─────────────────────────────────────────── */
/*
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
}
html {
  margin: 0;
  padding: 0;
}*/
.page-wrapper {
  --correct-color: #4caf50;
  --wrong-color: #f44336;
  --text-color: #333;
  --bg-color: #f4f7f6;
  --accent: #59b6c3;

  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: var(--bg-color);
  color: var(--text-color);
  font-size: 1.1rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* ── Header ────────────────────────────────────────────── */
.site-header {
  width: 100%;
  color: white;
  padding: 40px 10px;
  text-align: center;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  background-image: linear-gradient(to top, #6a85b6 0%, #bac8e0 100%);
}

.site-header h1 {
  margin: 0;
  font-size: 2.2rem;
  text-shadow: 2px 2px 2px #111111b2;
}

.subtitle {
  font-size: 21px;
  font-weight: normal;
  margin-bottom: 0;
  text-shadow: 2px 2px 2px #111111b2;
}

.avatar {
  border: 10px solid #ffffffa8;
  width: 250px;
  height: 250px;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: 4px 0px 16px #03090d17;
  display: block;
  margin: 0 auto 16px;
}

/* ── Main / Quiz ───────────────────────────────────────── */
.quiz-app {
  width: 90%;
  max-width: 1200px;
  background: white;
  margin: 30px 0;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.stats {
  font-weight: bold;
  margin-bottom: 20px;
  color: #555;
  text-align: right;
}

.score-value {
  color: var(--accent);
}

/* ── Pregunta ──────────────────────────────────────────── */
.question-container {
  margin-bottom: 30px;
}

.question-text {
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 20px;
}

/* ── Opciones ──────────────────────────────────────────── */
.options-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 12px;
}

@media (min-width: 768px) {
  .options-grid {
    grid-template-columns: 1fr 1fr;
  }
}

.option-btn {
  padding: 15px 20px;
  border: 2px solid #ddd;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.3s;
  font-size: 1rem;
  text-align: left;
}

.option-btn:hover:not(:disabled) {
  border-color: #7b7aad;
  background: #f0f0ff;
}

.option-btn.correct {
  background-color: var(--correct-color);
  color: white;
  border-color: var(--correct-color);
}

.option-btn.wrong {
  background-color: var(--wrong-color);
  color: white;
  border-color: var(--wrong-color);
}

/* ── Feedback ──────────────────────────────────────────── */
.feedback-box {
  margin-top: 20px;
  padding: 20px;
  border-radius: 8px;
  background-color: #e8f4fd;
  border-left: 5px solid #2196f3;
  animation: fadeIn 0.5s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* ── Botones de navegación ─────────────────────────────── */
.nav-btn {
  margin-top: 20px;
  padding: 12px 30px;
  border: none;
  border-radius: 0.5rem;
  background: var(--accent);
  color: white;
  font-size: 1.1rem;
  cursor: pointer;
  transition: background 0.2s;
}

.nav-btn:hover {
  background: #469fac;
}

/* ── Resultados ────────────────────────────────────────── */
.results {
  text-align: center;
  padding: 20px 0;
}

.retry-wrapper {
  text-align: center;
}

/* ── Footer ────────────────────────────────────────────── */
footer {
  font-weight: normal;
  color: #777;
  margin-bottom: 16px;
}
</style>
