<template>
    <div class="container" v-cloak>
        <div class="card">
            <div class="card-heading">
                <p>Advice {{ adviceNumber }}</p>
            </div>
            <div class="card-text">
                <p>{{ advice }}</p>
            </div>

            <div class="icon-divide">
                <img src="./assets/pattern-divider-desktop.svg" alt="" />
            </div>

            <div class="dice-icon-container" @click="generateQuote">
                <img src="./assets/icon-dice.svg" alt="" />
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Advice generator app",
    data() {
        return {
            adviceText: "",
            adviceNo: "",
        };
    },

    computed: {
        advice() {
            return "“" + this.adviceText + "”";
        },
        adviceNumber() {
            return "#" + this.adviceNo;
        },
    },
    async mounted() {
        const response = await axios.get("https://api.adviceslip.com/advice");
        this.adviceText = response.data.slip.advice;
        this.adviceNo = response.data.slip.id;
    },

    methods: {
        async generateQuote() {
            const response = await axios.get(
                "https://api.adviceslip.com/advice"
            );
            this.adviceText = response.data.slip.advice;
            this.adviceNo = response.data.slip.id;
        },
    },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@200;300;400;500;600;700;800&display=swap");

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    width: 100%;
    min-height: 100vh;
    background-color: #202632;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: "Manrope", sans-serif;
}

[v-cloak] {
    display: none;
}

.card {
    width: 500px;
    min-height: 100px;
    background-color: #313a49;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 55px;
    border-radius: 15px;
    position: relative;

    .card-heading {
        p {
            color: hsl(150, 100%, 66%);
            font-weight: 800;
            font-size: 12px;
            letter-spacing: 3px;
            text-transform: uppercase;
        }
    }

    .card-text {
        color: hsl(193, 38%, 86%);
        margin: 35px 0;
        font-size: 28px;
        font-weight: 800;
    }

    .icon-divide {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: row;
    }

    .dice-icon-container {
        position: absolute;
        bottom: 0;
        // transform: translate(-50%, 50%);
        transform: translateY(50%);
        width: 35px;
        height: 35px;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: hsl(150, 100%, 66%);
        padding: 28px;
        border-radius: 50%;
        cursor: pointer;
        transition: 0.5s;
    }

    .dice-icon-container:hover {
        box-shadow: 0px 0px 40px hsl(150, 100%, 66%);
    }
}

@media (max-width: 580px) {
    body {
        // max-height: 100%;
        // padding: 40px;
    }
    // .container {
    //     padding: 40px;
    // }
    .card {
        // min-width: 250px;
        width: 250px;
    }
}
</style>
