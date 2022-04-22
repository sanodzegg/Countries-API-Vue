<script setup>
  import Country from "./Country.vue"
</script>

<script>
    export default {
        data() {
            return {
                countriesToDisplay: [],
            }
        },
        mounted:function() {
            fetch(`https://restcountries.com/v3.1/all`).then(res => res.json())
            .then(data => {
                data.forEach((e,i) => {
                    if(i < 12) {
                        this.countriesToDisplay.push(e);
                    }
                });
            });
            console.log(this.countriesToDisplay);
        },
        methods: {

            
        }
    }
</script>

<template>
    <div class="filter__section">
        <input type="text" placeholder="Search for a country...">
        <select name="regionFilter" id="regionFilter">
            <option value="default" selected default disabled>Filter by Region</option>
            <option value="africa">Africa</option>
            <option value="america">America</option>
            <option value="asia">Asia</option>
            <option value="oceania">Oceania</option>
        </select>
    </div>
    <main aria-label="country list">
        <Country v-for="country in countriesToDisplay" :key="country" 
        :flag="country.flags.png" 
        :name="country.name.common" :population="country.population"
        :region="country.region" :capital="country.capital[0]" />
    </main>
</template>