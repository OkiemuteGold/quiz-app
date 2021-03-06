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
                    :disabled="answered"
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
            <b-button
                variant="primary"
                @click="nextQuestion"
                v-if="notLastQuestion"
                :disabled="!answered"
                >Next</b-button
            >
            <!-- <b-button variant="primary" @click="resetQuestion" v-else
                >Try again</b-button
            > -->
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
        // resetQuestion: Function,
        increment: Function,
        questions: Array,
        index: Number,
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,
            notLastQuestion: true,
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
            if (this.index + 1 == this.questions.length) {
                // console.log(this.index);
                // console.log(this.questions.length);
                // console.log("last");

                this.notLastQuestion = !this.notLastQuestion;
            }

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
                answerClass = "correct animateCorrectClass";
            } else if (
                this.answered &&
                this.selectedIndex === index &&
                this.correctIndex !== index
            ) {
                answerClass = "incorrect animateIncorrectClass";
            }
            return answerClass;
        },
    },
};
</script>

<style scoped>
.question-box-container {
    margin: 50px auto 0;
    max-width: 450px;
}
.jumbotron {
    padding: 2rem;
    width: 100%;
    color: #eee;
    background-color: rgba(0, 0, 0, 0.3);
    margin: auto;
}
.lead {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
.list-group {
    margin-bottom: 15px;
}
.list-group-item {
    margin-bottom: 5px;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.4s ease-in-out;
    letter-spacing: 0.5px;
    color: #222;
    border: none;
    font-weight: 400;
}
.list-group-item:not(.selected):hover {
    background-color: #d1e6eb;
}
.btn {
    margin: 0 5px;
    width: 90px;
    border-radius: 5px;
    transition: 0.4s ease;
}
.btn:focus,
.btn:active {
    box-shadow: none !important;
}

.selected {
    background: lightskyblue;
}
.correct,
.incorrect {
    font-weight: 700;
}
.correct {
    background: lightgreen;
}
.incorrect {
    background: red;
}

.animateCorrectClass,
.animateIncorrectClass {
    animation-duration: 1.3s;
    animation-fill-mode: forwards;
}
.animateCorrectClass {
    animation-name: animateCorrectClass;
}
.animateIncorrectClass {
    animation-name: animateIncorrectClass;
}

@keyframes animateCorrectClass {
    0%,
    35%,
    75% {
        font-weight: 400;
        background: lightgreen;
        color: #444;
    }

    25%,
    50%,
    90% {
        font-weight: 300;
        background: #ceffce;
        color: #888;
    }

    100% {
        font-weight: 700;
        background: lightgreen;
        color: #222;
    }
}

@keyframes animateIncorrectClass {
    0%,
    35%,
    75% {
        font-weight: 400;
        background: red;
        color: #444;
    }

    25%,
    50%,
    90% {
        font-weight: 300;
        background: #fa9a9a;
        color: #888;
    }

    100% {
        font-weight: 700;
        background: red;
        color: #222;
    }
}

@media screen and (max-width: 900px) {
    .question-box-container {
        margin: 35px auto 0;
    }
}
@media screen and (max-width: 500px) {
    .question-box-container {
        margin: 20px auto 0;
    }
}
@media screen and (max-width: 374px) {
    .jumbotron {
        padding: 1rem 1.5rem;
    }
    .lead {
        font-size: 18px;
    }
}
</style>