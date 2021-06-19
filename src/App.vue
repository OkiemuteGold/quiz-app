<template>
    <div id="app">
        <!-- <div id="nav">
            <router-link to="/">Home</router-link>
        </div>
        <router-view /> -->
        <Header />

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
            amount: 10,
            category: 21,
            url: `https://opentdb.com/api.php?amount=${this.amount}&category=${this.category}&type=multiple`,
            method: "get",
            questions: [],
            index: 0,
        };
    },
    methods: {
        next() {
            this.index += 1;
        },
    },
    mounted() {
        fetch(this.url, {
            method: this.method,
        })
            .then((resp) => {
                let response = resp.json();
                return response;
            })
            .then((data) => {
                this.questions = data.results;
            });
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
