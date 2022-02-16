<template>
  <main>
    <h2 class="title">Дорогие друзья, при выполнение заданий данного тренажёра помните о нескольких правилах для корректной проверки ваших ответов:</h2>
    <h2 class="title">1) Если вам требуется написать значок квадратного корня, то следует написать <i>корень</i>, и, после введения еще одного символа, слово <i>корень</i> заменится на значок корня</h2>
    <h2 class="title">2) Если вам нужно поставить минус перед корнем, то следует написать <i>-корень</i> и после этого необходимое число</h2>
    <h2 class="title">3) Целые числа рекомендуется писать со знаменателем 1</h2>
    <h2 class="title">4) Для проверки нажмите на кнопку "Узнать результаты"</h2>
    <ul class="exercise-list">
      <li v-for="exercise in exerciseList" :key="exercise.id">
        <Exercise :exercise="exercise" ></Exercise>
      </li>
      <li>
        <button @click="onShowResults" class="submit-button">Узнать результаты</button>
      </li>
    </ul>
  </main>
</template>

<script>
import Exercise from './components/Exercise.vue';
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    Exercise
  },
  setup() {
    const angleNumerators = [1, 1, 1, 1, 2, 3, 5, 1, 7, 5, 4, 3, 5, 7, 11, 0];
    const angleDenominators = [6, 4, 3, 2, 3, 4, 6, 1, 6, 4, 3, 2, 3, 4, 6, 1];
    const exerciseList = ref([]);
    
    for (let index = 0; index < 20; index++) {
      const n = Math.floor(Math.random() * 16);
      const newExercise = {
        n: n,
        func: Math.random() >= 0.5 ? 'sin' : 'cos',
        sign: angleNumerators[n] === 0 ? '' : (Math.random() >= 0.5 ? '-' : ''),
        angleNumerator: angleNumerators[n] === 0 ? '0' : (angleNumerators[n] === 1 ? 'π' : angleNumerators[n] + 'π'),
        angleDenominator: angleDenominators[n] === 1 ? '' : '/' + angleDenominators[n],
        answerNum: '',
        answerDen: '',
        correct: 0,
        id: index
      }
      exerciseList.value = [...exerciseList.value, newExercise]
    }

    const onShowResults = () => {
      const resultList = [];

      console.log(exerciseList)

      exerciseList.value.map(exercise => {
        const numSign = exercise.answerNum[0] === '-' ? -1 : 1;
        const num = exercise.answerNum[0] === '-' ? exercise.answerNum.slice(1, exercise.answerNum.length) : exercise.answerNum;
        const denSign = exercise.answerDen[0] === '-' ? -1 : 1;
        const den = exercise.answerDen[0] === '-' ? exercise.answerDen.slice(1, exercise.answerDen.length) : exercise.answerDen;
        const finalNum = num[0] === '√' ? numSign * Math.sqrt(Number(num.slice(1))) : numSign * Number(num);
        const finalDen = den[0] === '√' ? denSign * Math.sqrt(Number(den.slice(1))) : denSign * Number(den);
        const angle = angleNumerators[exercise.n] * Math.PI / angleDenominators[exercise.n];
        const signedAngle = exercise.sign === '' ? angle : -1 * angle;
        const answer = exercise.func === 'sin' ? Math.sin(signedAngle) : Math.cos(signedAngle);
        const yourAnswer = finalNum / finalDen;
        exerciseList.value[exercise.id].correct = +yourAnswer.toFixed(6) + 0.000001 === +answer.toFixed(6) + 0.000001 ? 1 : -1;
      })
    }

    return{
      exerciseList,
      onShowResults
    }
  }
}


</script>

<style>
  .exercise-list {
    list-style: none;
    margin-top: 10px;
  }
</style>