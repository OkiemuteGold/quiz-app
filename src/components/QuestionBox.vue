<template>
    <div class="question_box_container">
        <b-jumbotron
            header="BootstrapVue"
            lead="Bootstrap v4 Components for Vue.js 2"
        >
            <template #lead> Some Questions </template>

            <hr class="my-4" />

            <p>List of answers</p>

            <b-button variant="primary" href="#">Submit</b-button>
            <b-button variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default {
    name: "QuestionBox",
    components: {},
};
</script>

<style>
</style>