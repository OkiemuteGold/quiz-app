<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>
                <h1>
                    <b>Maths Quiz App</b>
                </h1>
            </b-nav-item>
            <b-nav-item>
                Answered:
                <span>{{ numCorrect }}</span> out of <span>{{ numTotal }}</span>
            </b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    name: "Header",
    components: {},
    props: {
        numCorrect: Number,
        numTotal: Number,
    },
};
</script>

<style scoped>
.nav {
    align-items: center;
    padding: 10px 20px;
}
.nav-tabs .nav-link {
    border: none;
    cursor: auto;
    color: #fff;
}

.nav-tabs .nav-link span {
    color: #3a89ff;
    background: #fff;
    font-weight: 600;
    padding: 2px 5px;
}
.nav-tabs .nav-link span:first-child {
    color: lightgreen;
}

/* .nav-tabs .nav-link:hover {
    border: none;
    color: #0d6efd;
} */
.nav-tabs .nav-link.disabled {
    color: #eee;
}
h1 {
    font-size: 1.5em;
}

@media screen and (max-width: 768px) {
    .nav {
        justify-content: space-around;
    }
}
@media screen and (max-width: 500px) {
    .nav {
        flex-direction: column;
    }
    .nav-link {
        padding: 0.3rem 1rem;
    }
}
/* @media only screen and (width: 360px) {
    .nav {
        margin-top: 10px;
    }
} */
</style>