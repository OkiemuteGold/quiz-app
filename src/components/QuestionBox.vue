<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4" />

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers"
                    :key="index"
                    @click.prevent="selectedAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button
                variant="success"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
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
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
        };
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            },
        },
    },
    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index;
        },
        submitAnswer() {
            let isCorrect = false;
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        shuffleAnswers() {
            let answers = [
                ...this.currentQuestion.incorrect_answers,
                this.currentQuestion.correct_answer,
            ];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(
                this.currentQuestion.correct_answer
            );
        },
        answerClass(index) {
            let answerClass = "";
            if (!this.answered && this.selectedIndex === index) {
                answerClass = "selected";
            } else if (this.answered && this.correctIndex === index) {
                answerClass = "correct";
            } else if (
                this.answered &&
                this.selectedIndex === index &&
                this.correctIndex !== index
            ) {
                answerClass = "incorrect";
            }
            return answerClass;
        },
    },
};
</script>

<style scoped>
.question-box-container {
    margin-top: 20px;
}
.jumbotron {
    padding: 2rem;
    width: 100%;
    background-color: #f2f2f2;
    margin: auto;
}
.lead {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.list-group {
    margin-bottom: 20px;
}
.list-group-item {
    margin-bottom: 5px;
    cursor: pointer;
    transition: 0.4s ease;
}
.list-group-item:not(.selected):hover {
    background-color: #edf5f7;
}
.btn {
    margin: 0 5px;
    width: 90px;
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