<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled>
                <b>My Quiz App</b>
            </b-nav-item>
            <b-nav-item>
                Counter:
                <span>{{ numCorrect }}</span> / <span>{{ numTotal }}</span>
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

<style>
</style>