<template>
    <div id="app">
        <!-- <div id="nav">
            <router-link to="/">Home</router-link>
        </div>
        <router-view /> -->
        <Header />

        <div
            id="error"
            v-if="questions.length == 0"
            :class="{ showError: questions.length == 0 }"
        ></div>

        <b-container class="bv-example-row">
            <b-row>
                <b-col sm="6" offset="3">
                    <QuestionBox
                        v-if="questions.length"
                        :currentQuestion="questions[index]"
                        :nextQuestion="next"
                    />
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
    name: "app",
    components: {
        Header,
        QuestionBox,
    },
    data() {
        return {
            questions: [],
            index: 0,
            // amount: 10,
            // category: 21,
        };
    },
    methods: {
        next() {
            this.index += 1;
        },
        fetchQuestion() {
            let amount = 10;
            let category = 21;
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
    },
    mounted() {
        // url format: https://opentdb.com/api.php?amount=15&category=19&difficulty=medium&type=multiple
        // category 9 = general knowledge, 19 = mathematics, 21 = sports, 27 = animals
        // type=boolean or multiple

        this.fetchQuestion;
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

.show-error {
    background: #f00;
    color: #fff;
}

#error {
    margin: 20px auto;
    padding: 20px 10px;
    border-radius: 5px;
}

#error p {
    margin: 0;
}

/* #nav {
    padding: 30px;
}

#nav a {
    font-weight: bold;
    color: #2c3e50;
}

#nav a.router-link-exact-active {
    color: #42b983;
} */
</style>
