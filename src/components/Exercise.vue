<template>
    <div class="exercise-card my-style" v-bind:class="exercise.correct === -1 ? 'incorrect' : (exercise.correct === 1 ? 'correct': '')">
        <div>
            <h3>{{exercise.func}}({{exercise.sign}}{{exercise.angleNumerator}}{{exercise.angleDenominator}}) = </h3>
        </div>
        <div>
            <input type="text" v-model="exercise.answerNum" @keyDown="onTextChange(exercise)"/>
        </div>
        <div>
            <h3>/</h3>
        </div>
        <div>
            <input type="text" v-model="exercise.answerDen"  @keyDown="onTextChange(exercise)"/>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
    props: {
        exercise: {
            required: true,
            default: {
                n: 2, 
                func: 'sin',
                sign: '',
                angleNumerator: 'π',
                angleDenominator: '/4',
                answerNum: '',
                answerDen: '',
                correct: 0,
                id: 0
            }
        }
    },
    methods: {
        onTextChange(a) {
            a.answerNum = a.answerNum ? a.answerNum.toLowerCase().replace('корень', '√') : a.answerNum
            a.answerDen = a.answerDen ? a.answerDen.toLowerCase().replace('корень', '√') : a.answerDen
        }
    }
}
</script>

<style>
    .exercise-card{
        display: flex;
        justify-content: space-between;
        align-items: center;
        max-width: 400px;
        margin: 0 auto;
        background-color: lightgrey;
    }
    .incorrect{
        background-color: #FADADD
    }
    .correct{
        background-color: #98ff98
    }
</style>