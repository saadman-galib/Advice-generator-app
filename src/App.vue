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
                <img v-bind:src="dividerImg" alt="" />
            </div>

            <div class="dice-icon-container" @click="generateQuote">
                <img
                    src="/assets/icon-dice.svg"
                    alt=""
                    class="dice-img"
                    :style="{ transform: 'rotate(' + turn + 'deg)' }"
                />
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
            dividerImg: "/assets/pattern-divider-desktop.svg",
            window: {
                width: 0,
                height: 0,
            },
            turn: 1080,
        };
    },

    created() {
        window.addEventListener("resize", this.handleResize);
        this.handleResize();
    },

    destroyed() {
        window.removeEventListener("resize", this.handleResize);
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

        console.log(this.window.width);
        console.log(typeof this.window.height);

        if (this.window.width <= 580) {
            this.dividerImg = "/assets/pattern-divider-mobile.svg";
        } else {
            this.dividerImg = "/assets/pattern-divider-desktop.svg";
        }
    },

    methods: {
        async generateQuote() {
            this.turn = this.turn + 1080;

            const response = await axios.get(
                "https://api.adviceslip.com/advice"
            );
            this.adviceText = response.data.slip.advice;
            this.adviceNo = response.data.slip.id;
        },

        handleResize() {
            this.window.width = window.innerWidth;
            this.window.height = window.innerHeight;
        },

        // turnAround(){
        //     this.turn = this.turn + 1080;
        // }
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

    .dice-icon-container:hover img {
        transition: 2s;
    }
}

@media (max-width: 580px) {
    body {
        // max-height: 100%;
        // padding: 40px;
        width: 100%;
        height: auto;
        // margin: auto;
        padding: auto;
    }
    .container {
        padding: 25px;
        margin: auto;
        width: 100%;
    }
    .card {
        // min-width: 250px;
        max-width: 450px;
        margin-left: auto;
    }
}

@media (max-width: 500px) {
    .card {
        max-width: 400px;
    }
}

@media (max-width: 450px) {
    .card {
        max-width: 350px;
    }
}

@media (max-width: 400px) {
    .container {
        padding: 10px;
    }
    .card {
        max-width: 300px;
    }
}

@media (max-width: 320px) {
    .container {
        padding: 2px;
    }
    .card {
        max-width: 295px;
    }
}
</style>
