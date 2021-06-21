<template>
    <div id="app" class="bg-dark">
        <div class="container">
            <Header :numCorrect="numCorrect" :numTotal="numTotal" />

            <div
                id="error"
                v-if="questions.length == 0"
                :class="{ showError: questions.length == 0 }"
            ></div>

            <b-container class="bv-example-row">
                <b-row>
                    <b-col md="6" offset-md="3">
                        <QuestionBox
                            v-if="questions.length"
                            :currentQuestion="questions[index]"
                            :nextQuestion="next"
                            :increment="increment"
                        />
                    </b-col>
                </b-row>
            </b-container>

            <Footer />
        </div>
    </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import Footer from "./components/Footer.vue";

export default {
    name: "app",
    components: {
        Header,
        QuestionBox,
        Footer,
    },
    data() {
        return {
            questions: [],
            index: 0,
            numCorrect: 0,
            numTotal: 0,
        };
    },
    methods: {
        next() {
            this.index++;
        },
        increment(isCorrect) {
            if (isCorrect) {
                this.numCorrect++;
            }
            this.numTotal++;
        },
    },
    mounted() {
        // url format: https://opentdb.com/api.php?amount=15&category=19&difficulty=medium&type=multiple
        // category 9 = general knowledge, 19 = mathematics, 21 = sports, 27 = animals
        // type=boolean or multiple

        let amount = 40;
        let category = 19;
        let url = `https://opentdb.com/api.php?amount=${amount}&category=${category}&type=multiple`;
        const method = "get";
        fetch(url, {
            method,
        })
            .then((response) => {
                return response.json();
            })
            .then((data) => {
                this.questions = data.results;
            })
            .catch((error) => {
                if (error) {
                    let p = document.createElement("p");
                    p.innerHTML =
                        "Failed to load question due to poor internet connection. Please try again!";
                    let errorDiv = document.getElementById("error");
                    errorDiv.appendChild(p);
                }
            });
    },
};
</script>

<style>
#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #222;
    overflow-x: hidden;
    height: 100vh;
}
.showError {
    background: #f00;
    color: #fff;
}
#error {
    width: 600px;
    margin: 20px auto;
    border-radius: 5px;
}
#error p {
    margin: 0;
    padding: 10px;
}
</style>
