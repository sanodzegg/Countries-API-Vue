<script setup>
    import { reactive } from 'vue'
</script>
<script>
    export default {
        props : {
            flag: String,
            name: String,
            nativeName: Object,
            population: String,
            region: String,
            subRegion: String,
            capital: String,
            currencies: Object,
            languages: Object,
            borders: Array
        },
        methods: {
            showHide() {
                display.displayCountries = true;
                display.countryDetails = false;
            }
        }
    };
    export const display = reactive({
        displayCountries: true,
        countryDetails: false,
    });
</script>
<template>
    <button class="back__button" v-on:click="showHide">← Back</button>
    <div class="country__details-wrapper">
        <img :src="flag" alt="country flag">
        <div class="coutry__details">
            <h1 class="country__name">{{ name }}</h1>
            <div class="details__wrapper">
                <div class="details__l">
                    <h2 v-for="i in nativeName" :key="i"><span>Native Name:</span> {{ i.official }}</h2>
                    <h2><span>Population:</span> {{ population }}</h2>
                    <h2><span>Region:</span> {{ region }}</h2>
                    <h2><span>Sub Region:</span> {{ subRegion }}</h2>
                    <h2><span>Capital:</span> {{ capital }}</h2>
                </div>
                <div class="details__r">
                    <h2 v-for="i in currencies" :key="i"><span>Currencies:</span> {{ i.name }}</h2>
                    <h2 v-for="i in languages" :key="i"><span>Languages:</span> {{ i }}</h2>
                </div>
            </div>
            <div class="details__b">
                <div class="borders__wrapper">
                    <span>Border Countries:</span>
                    <button v-for="i in borders" :key="i">{{ i }}</button>
                </div>
            </div>
        </div>
    </div>
    
</template>