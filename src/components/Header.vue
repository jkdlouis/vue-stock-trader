<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
            <router-link to="/" class="navbar-brand">Stock Trader</router-link>

        <div class="collapse navbar-collapse">
            <ul class="navbar-nav mr-auto">
                <router-link to="/portfolio" activeClass="active" tag="li" class="mr-3"><a>Portfolio</a></router-link>
                <router-link to="/stocks" activeClass="active" tag="li"><a>Stocks</a></router-link>
            </ul>
        </div>
        <ul class="nav navbar-nav mr-3">
            <li class="mr-3"><a role="button" @click="endDay">End Day</a></li>
            <li class="dropdown">
                <a href="#"
                   class="dropdown-toggle"
                   data-toggle="dropdown"
                   role="button"
                   @click="isDropdownOpen = !isDropdownOpen"
                   :aria-haspopup="isDropdownOpen"
                   :aria-expanded="!isDropdownOpen">Save & Load<span class="caret"></span></a>
                <ul class="dropdown-menu" :class="{ show: isDropdownOpen }">
                    <li class="dropdown-item"><a role="button" @click="saveData">Save Data</a></li>
                    <li class="dropdown-item"><a role="button" @click="loadData">Load Data</a></li>
                </ul>
            </li>
        </ul>
        <strong class="navbar-text">Funds: {{ funds | currency }}</strong>
    </nav>
</template>

<script>
    import { mapActions } from 'vuex';

    export default {
        name: "Header",
        data() {
            return {
                isDropdownOpen: false
            }
        },
        computed: {
            funds() {
                return this.$store.getters.funds;
            }
        },
        methods: {
            ...mapActions({
                randomizeStocks: 'randomizeStocks',
                fetchData: 'loadData'
            }),
            endDay() {
                this.randomizeStocks();
            },
            saveData() {
                const data = {
                    funds: this.$store.getters.funds,
                    stockPortfolio: this.$store.getters.stockPortfolio,
                    stocks: this.$store.getters.stocks
                };
                this.$http.put('data.json', data);
            },
            loadData() {
              this.fetchData();
            }
        }
    }
</script>

<style scoped>

</style>