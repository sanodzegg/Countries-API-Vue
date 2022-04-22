<script>
    export default {
        props : {
            flag: String,
            name: String,
            population: Number,
            region: String,
            capital: String
        }
    };
</script>

<template>
    <div class="country__wrapper">
        <div class="country__img-wrapper">
            <img class="country__img" :src="flag" alt="country flag">
        </div>
        <div class="country__info-wrapper">
            <h1>{{ name }}</h1>
            <h2><span>Population:</span> {{ population }}</h2>
            <h2><span>Region:</span> {{ region }}</h2>
            <h2><span>Capital:</span> {{ capital }}</h2>
        </div>
    </div>
</template>