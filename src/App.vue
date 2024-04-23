<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == "" ? "..." : cityName }}</p>
        <input type="text" v-model="city" placeholder="Введите свой город">
        <button v-if="city != ''" @click="GetWeather()">Узнать погоду</button>
        <button disabled v-else>Введите название города(En)</button>
        <p class="error">{{ error }}</p>
        <div v-if="info != null">
            <p>{{ showTemp }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            city: "",
            error: "",
            info: null,
        }
    },
    computed: {
        cityName() {
            return '«' + this.city + '»'
        },
        showTemp() {
            return "Температура:" + this.info.main.temp
        },
        showFeelsLike() {
            return "Ощущается как:" + this.info.main.feels_like
        },
        showMinTemp() {
            return "Минимальная температура:" + this.info.main.temp_min
        },
        showMaxTemp() {
            return "Максимальная Температура:" + this.info.main.temp_max
        },

    },
    methods: {
        GetWeather() {
            if (/^\d+$/.test(this.city.trim())) {
                this.error = "Город не может состоять только из цифр";
                return false;
            }

            this.error = ""

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=397c7919275c355bd507b2a5bfc29c4f`)
                .then(res => (this.info = res.data))
        }
    }
}
</script>

<style scoped>
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    background-color: rgba(77, 22, 77, 0.589);
    padding: 20px;
    text-align: center;
    color: white;
}

.wrapper h1 {
    margin-top: 50px;
}

.wrapper p {
    margin-top: 20px;
}

.wrapper input {
    margin-top: 30px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid rgb(29, 8, 8);
    color: white;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: rgb(156, 7, 156);
}

.wrapper button {
    background: #E3BC4B;
    color: white;
    border-radius: 10px;
    border: 2px solid #b99935;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-5px);
}

button:disabled {
    background: #e3bd4b77;
    border: 2px solid #b998357c;
    cursor: not-allowed;
}

button:disabled:hover {
    transform: none
}

.error {
    color: red;
}
</style>
