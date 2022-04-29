<script setup>
    import Country from "./Country.vue"
    import CountryDetailed from "./CountryDetailed.vue"
    import { display } from "./CountryDetailed.vue"
</script>

<script>
    export default {
        data() {
            return {
                countriesToDisplay: [],
                countryInfoDisplay: [],
                searchTerm: "",

                startpoint: 12,
                endpoint: 11,
            }
        },
        mounted:function() {
            this.getCountries();
            display.countryDetails = false;

            this.getTheme();
            delete this.getTheme();
        },
        methods: {
            getCountries(event) {
                if(event) {
                    this.countriesFetch(`https://restcountries.com/v3.1/region/${event.target.value}`);
                } else {
                    this.countriesFetch(`https://restcountries.com/v3.1/all`);
                }
            },
            countriesFetch(url) {
                fetch(url).then(res => res.json())
                .then(data => {
                    data.forEach((e, i) => {
                        if(i <= this.endpoint) {
                            e.population = e.population.toString().replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1.");
                            this.countriesToDisplay.splice(i, i+1, e);
                        }
                        this.getNextCountries(data);
                    });
                });
            },
            getNextCountries(data) {
                window.onscroll = () => {
                    let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
                    if (bottomOfWindow) {
                        this.endpoint += 12;
                        for(let i = this.startpoint; i < data.length; i++) {
                            if(i < this.endpoint && data[i].capital != undefined) {
                                data[i].population = data[i].population.toString().replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1.");
                                this.countriesToDisplay.push(data[i]);
                            }
                        }
                        this.startpoint += 12;
                    }
                };
            },
            displayCountry(type, event, name) {
                if(type == 'click') {
                    name = event.composedPath()[2].children[1].firstChild.innerText;
                } else {
                    name = name;
                }
                fetch(`https://restcountries.com/v3.1/name/${name}`).then(res => res.json())
                .then(data => {
                    let parsedPopulation = data[0].population.toString();
                    parsedPopulation = parsedPopulation.replace(/(\d)(?=(\d\d\d)+(?!\d))/g, "$1.");
                    let obj = {
                        flag: data[0].flags.png,
                        name: name,
                        nativeName: data[0].name.nativeName,
                        population: parsedPopulation,
                        region: data[0].region,
                        subregion: data[0].subregion,
                        capital: data[0].capital[0],
                        currency: data[0].currencies,
                        languages: data[0].languages,
                        borders: data[0].borders
                    }
                    this.countryInfoDisplay.splice(0, 1, obj);
                    display.countryDetails = true;
                    display.displayCountries = false;
                });
            },
            searchCountry(e) {
                if(e.keyCode === 13) {
                    fetch(`https://restcountries.com/v3.1/name/${this.searchTerm}`).then(res => res.json())
                    .then(() => {
                        this.displayCountry('search', null, this.searchTerm);
                    });
                }
            },
            getTheme() {
                let theme = localStorage.getItem("user-theme");
                if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
                    localStorage.setItem("user-theme", "dark");
                    document.body.className = theme;
                } else {
                    localStorage.setItem("user-theme", "white");
                    document.body.className = theme;
                }
            },
        }
    }
</script>

<template>
    <div class="filter__section">
        <input type="text" placeholder="Search for a country..." v-bind:value="searchTerm" v-on:input="searchTerm = $event.target.value" v-on:keyup="searchCountry">
        <select name="regionFilter" id="regionFilter" v-on:change="getCountries($event)">
            <option disabled selected value="">Filter by Region</option>
            <option value="africa">Africa</option>
            <option value="america">America</option>
            <option value="asia">Asia</option>
            <option value="oceania">Oceania</option>
        </select>
    </div>
    <main aria-label="country list" v-if="display.displayCountries">
        <Country v-for="country in countriesToDisplay" :key="country"
        :flag="country.flags.png" 
        :name="country.name.common" :population="country.population"
        :region="country.region" :capital="country.capital[0]" v-on:click="displayCountry('click', $event, null)" />
    </main>
    
    <section class="country__details-section" v-if="display.countryDetails">
        <CountryDetailed v-for="item in countryInfoDisplay" :key="item"
            :flag="item.flag" :name="item.name" :nativeName="item.nativeName" :population="item.population"
            :region="item.region" :subRegion="item.subregion" :capital="item.capital" :currencies="item.currency"
            :languages="item.languages" :borders="item.borders"
        />
    </section>
</template>