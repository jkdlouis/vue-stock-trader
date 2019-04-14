<template>
    <div class="col-sm-6 col-md-4 mt-5">
        <div class="card">
            <div class="card-header bg-success">
                <h3 class="card-title">
                    {{ stock.name }} <small>(price: {{ stock.price }})</small>
                </h3>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col">
                        <input type="number"
                               class="form-control"
                               placeholder="Quantity"
                               v-model.number="quantity"
                               :class="{ danger: insufficientFunds }"/>
                    </div>
                    <div class="col-auto">
                            <button class="btn btn-success"
                                    @click="buyStock"
                                    :disabled="insufficientFunds || quantity <= 0 || !Number.isInteger(quantity)">
                                {{ insufficientFunds ? 'Insufficient Funds' : 'Buy' }}
                            </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Stock",
        props: ['stock'],
        data() {
            return {
                quantity: 0
            }
        },
        computed: {
           funds() {
               return this.$store.getters.funds;
           },
           insufficientFunds() {
               return this.quantity * this.stock.price > this.funds
           }
        },
        methods: {
            buyStock() {
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.quantity
                };
                this.$store.dispatch('buyStock', order);
                this.quantity = 0;
            }
        }
    }
</script>

<style scoped>
  .danger {
      border: 1px solid red;
  }
</style>