<template>
    <div class="question_box_container">
        <b-jumbotron class="bg-light">
            <template #lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4" />

            <!-- <p>List of answers</p> -->
            <ul>
                <li v-for="(answer, index) in answers" :key="index">
                    {{ answer }}
                </li>
            </ul>

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
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        },
    },
};
</script>

<style scoped>
.jumbotron {
    padding: 4rem 2rem;
}
button:first-child {
    margin-right: 5px;
}
</style>