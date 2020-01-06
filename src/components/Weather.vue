<template>
    <div class="hello">
        <h1>Weather in {{ currentCity }} </h1>
        <form @submit.prevent="getData" action="/" class="">
            <input class="field-city" v-model="city" type="text">
            <select class="field-country" v-model="country">
                <option value="ua">UA</option>
                <option value="usa">USA</option>
            </select>
            <button class="btn" type="submit">Submit</button>
        </form>
        <template v-if="list">
            <div class="weather-list">
                <ul class="list">
                    <li class="list__item" v-for="(list, index) in list" :key="index">
                        <h2 class="title--secondary"> {{ getDate(list) }} </h2>
                        <h3 class="title--third-level">{{ getTime(list) }}</h3>
                        <div>
                            <p> Min temperature: {{ list.main.temp_min }}</p>
                            <p> Max temperature: {{ list.main.temp_max }}</p>
                            <figure>
                                <img class="list__icon" v-bind:src="'http://openweathermap.org/img/wn/' + list.weather[0].icon + '.png'">
                                <figcaption>{{ list.weather[0].description}}</figcaption>
                            </figure>
                        </div>
                    </li>
                </ul>
            </div>
        </template>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        data: () => ({
            country: "ua",
            city: "Cherkasy",
            list: "",
            countryCode: "ua",
            cityCore: "",
            currentCity: ""
        }),
        methods: {
            getData: function () {
                let cityCore = this.city;
                let countryCode = this.country;

                axios.get('https://api.openweathermap.org/data/2.5/forecast?q=' + cityCore + ',' + countryCode + '&mode=json&appid=93478a40598608dceafd89ecb2584e95')
                    .then(response => {
                        this.currentCity = response.data.city.name;
                        this.list = response.data.list;
                    });
            },
            getDate: function (list) {
                return new Date(list.dt_txt).toLocaleDateString('uk-UA');
            },

            getTime: function (list) {
                return new Date(list.dt_txt).toLocaleTimeString('uk-UA');
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .title--secondary {
        font-weight: bold;
        margin-top: 0;
        margin-bottom: 10px;
    }

    .title--third-level {
        font-weight: bold;
        margin-top: 0;
        margin-bottom: 25px;
        margin-top: 0px;
    }

    .list__item {
        display: inline-block;
        padding: 20px;
        margin: 20px;
        border-radius: 10px;
        box-shadow: 0 10px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
        width: 200px;
    }

    .field-city {
        padding: 10px;
        border-radius: 10px;
        border: 1px solid lightgrey;
        color: darkslategrey;
        margin-right: 10px;
    }

    .field-country {
        padding: 10px;
        border-radius: 10px;
        border: 1px solid lightgrey;
        color: darkslategrey;
        margin-right: 10px;
        background-color: white;
    }

    .btn {
        padding: 10px;
        border-radius: 10px;
        border: 1px solid lightgrey;
        color: white;
        margin-right: 10px;
        background-color: darkslategrey;
        cursor: pointer;
    }

    .list__icon {
        margin: 0 auto;
    }

    @media (min-width: 1200px) {
        .list {
            max-width: 70%;
            margin: 0 auto;
            margin-top: 50px;
        }
    }

</style>
