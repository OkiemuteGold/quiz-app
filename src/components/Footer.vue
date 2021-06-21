<template>
    <div>
        <footer>
            <!-- rel="noopener" or rel="noreferrer" -->
            <p>
                &copy; <span id="footer-year"></span>
                <a
                    href="https://github.com/OkiemuteGold"
                    rel="noopener noreferrer"
                    target="_blank"
                    >Okiemute Gold</a
                >
            </p>
        </footer>
    </div>
</template>

<script>
export default {
    name: "Footer",
    mounted() {
        let footerYear = document.getElementById("footer-year");
        let fullCopyrightYear;
        let date = new Date();
        let currentYear = date.getFullYear();
        function getFooterYear() {
            currentYear > 2021
                ? (fullCopyrightYear = `2021 - ${currentYear}`)
                : (fullCopyrightYear = currentYear);
            footerYear.innerHTML = fullCopyrightYear;
        }
        getFooterYear();
    },
};
</script>

<style scoped>
footer {
    text-align: center;
    padding: 10px 20px 20px;
    font-size: 14px;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    margin: 10px auto 0;
    color: #eee;
}
footer p {
    margin-bottom: 0;
}
footer a {
    margin-left: 3px;
    color: #eee;
    transition: 0.4s ease;
}
footer a:hover {
    text-decoration: none;
}

@media screen and (max-width: 425px) {
    footer {
        padding-bottom: 5px;
    }
}
/* @media screen and (min-width: 370px) and (max-width: 423px) {
    footer {
        padding: 10px 20px;
    }
} */
</style>