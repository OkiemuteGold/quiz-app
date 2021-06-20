<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4" />

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click="selectedAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button
                variant="success"
                @click="submitAnswer"
                :disabled="selectedIndex === null || isAnswered"
            >
                Submit
            </b-button>
            <b-button variant="primary" @click="nextQuestion">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from "lodash";

export default {
    name: "QuestionBox",
    components: {},
    props: {
        currentQuestion: Object,
        nextQuestion: Function,
        increment: Function,
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            isAnswered: false,
        };
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        },
    },
    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index;
        },
        submitAnswer() {
            let isCorrect = false;
            if (this.selectedIndex === this.currentQuestion.correct_answer) {
                isCorrect = true;
            }
            this.isAnswered = true;
            this.increment(isCorrect);
        },
        shuffleAnswers() {
            let answers = [
                ...this.currentQuestion.incorrect_answers,
                this.currentQuestion.correct_answer,
            ];
            this.shuffledAnswers = _.shuffle(answers);
        },
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.isAnswered = false;
                this.shuffleAnswers();
            },
        },
    },
};
</script>

<style scoped>
.question-box-container {
    min-width: 400px;
    margin-top: 20px;
}
.jumbotron {
    padding: 4rem 3rem;
    width: 100%;
    background-color: #f2f2f2;
}
.lead {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.list-group {
    margin-bottom: 20px;
}
.list-group-item {
    cursor: pointer;
}
.list-group-item:not(.selected):hover {
    background-color: #eee;
}
.btn {
    margin: 0 5px;
    width: 100px;
}
.btn:focus,
.btn:active {
    box-shadow: none !important;
}
.selected {
    background-color: lightblue;
}
.correct {
    background-color: lightgreen;
}
.incorrect {
    background-color: #f00;
}
</style>