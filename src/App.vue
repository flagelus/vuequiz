<script setup>
import { ref, computed } from 'vue'
const questions = ref([
  {
	question: 'Oque é Vue?',
	answer: 0,
	options: [
		'Um framework',
		'Uma library',
		'Um pedaço de sapato'
	],
	selected: null
  },
  {
	question: 'Praque Vuex é utilizado?',
	answer: 2,
	options: [
		'É de comer',
		'Ver as coisas no app',
		'Manejamento de Estados'
	],
	selected: null
  },
  {
	question: 'Oque é Vue Router?',
	answer: 1,
	options: [
		'Um roteador de internet',
		'Uma livraria de rotas',
		'Hamburguer'
	],
	selected: null
  }
])
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})
const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})
const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}
const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
<main class = "app">
  <h1>Prova de Vue</h1>
  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{ getCurrentQuestion.question }}</span>
      <span class="score"> Pontos: {{ score }}/{{ questions.length }}</span>
    </div>

    <div class="options">
      
      <label v-for="(option, index) in getCurrentQuestion.options" :key="index"
       :class="`option ${
          getCurrentQuestion.selected == index? index == getCurrentQuestion.answer
        ?'correto':'errado':''
        
        }${
          getCurrentQuestion.selected != null && index != getCurrentQuestion.selected
          ?'disabled' :''
        }
        `">

        <input type="radio" :name="getCurrentQuestion.index" :vaLue="index" 
        v-model="getCurrentQuestion.selected" :disabled="getCurrentQuestion.selected"
        @change = "SetAnswer">
        <span>{{ option }}</span> 

      </label>
    </div>
        <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
              {{ getCurrentQuestion.index == questions.length - 1 ? 'Terminado'
              :getCurrentQuestion.selected == null ? 'Selecione uma Opção':'Próxima pergunta' }}
        </button>
  </section>
  <section v-else>
      <h2>Você terminou!🎊</h2>
      <p>Sua pontuação é {{ score }} / {{ questions.length }}</p>
  </section>
  </main>
</template>

<style>
*{
  margin: 0;
  padding: 0 ;
  box-sizing: border-box;
  font-family:'Montserrat', sans-serif;

}
body{
  background-color: #271c36;
background-image: linear-gradient(135deg, #271c36 0%, #4d3d67 100%);

  color: #fff;

}
.app{
  display:flex;
  flex-direction: column;
  align-items: center;
  padding:2rem;
  min-height: 100vh;

}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #382b4b;
  padding:1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}
.quiz-info{
  display:flex;
  justify-content: space-between;
  margin-bottom:1rem;

}
.quiz-info .question{
  color: #8f8f8f;
  font-size: 1.5rem;

}
.quiz-info .score{
  color:#fff;
  font-size:1.25rem;

}
.options{
  margin-bottom: 1rem;

}
.option{
  padding: 1rem;
  display:block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor:pointer;
  
}
.option:hover{
  background-color: #302144;
}
.option.correto{
  background-color: #2cee7d;
}
.option.errado{
  background-color: rgb(201, 33, 33);
}
.option:last-of-type{
  margin-bottom: 0;
}
.option.disabled{
  opacity:0.5;

}
.option input{
  display:none;

}
button{
  appearance:none;
  outline:none;
  border:none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cee7d;
  color:#2d213f;
  font-weight: 500;
  text-transform: uppercase;
  font-size: 1.05rem;
  border-radius:0.5rem;



}
button:disabled{
  opacity:0.5;

}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color:#8f8f8f;
  font-size: 1.30rem;
  text-align: center;

}
</style>
