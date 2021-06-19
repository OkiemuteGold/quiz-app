<template>
    <div class="question-box-container">
        <b-jumbotron class="bg-light">
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4" />

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in answers"
                    :key="index"
                    @click="selectedAnswer(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="success" href="#">Submit</b-button>
            <b-button variant="primary" href="#" @click="nextQuestion"
                >Next</b-button
            >
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    name: "QuestionBox",
    components: {},
    props: {
        currentQuestion: Object,
        nextQuestion: Function,
    },
    data() {
        return {
            selectedIndex: null,
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
    },
};
</script>

<style scoped>
.jumbotron {
    padding: 4rem 2rem;
}
.list-group {
    margin-bottom: 15px;
}

.list-group-item:hover {
    cursor: pointer;
    background: #eee;
}

.btn {
    margin: 0 5px;
}
</style>