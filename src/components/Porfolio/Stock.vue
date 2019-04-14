<template>
    <div class="col-sm-6 col-md-4 mt-5">
        <div class="card">
            <div class="card-header bg-info">
                <h3 class="card-title">
                    {{ stock.name }} <small>(price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small>
                </h3>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col">
                        <input type="number"
                               class="form-control"
                               placeholder="Quantity"
                               v-model.number="quantity"
                               :class="{ danger: insufficientQuantity }"/>
                    </div>
                    <div class="col-auto">
                        <button class="btn btn-info"
                                @click="sellStock"
                                :disabled="insufficientQuantity || quantity <= 0 || !Number.isInteger(quantity)">
                            {{ insufficientQuantity ? 'Not enough stocks' : 'Sell' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { mapActions } from 'vuex';

    export default {
        name: "Stock",
        props: ['stock'],
        data() {
            return {
                quantity: 0
            }
        },
        computed: {
          insufficientQuantity() {
              return this.quantity > this.stock.quantity;
          }
        },
        methods: {
            ...mapActions({
                placeSellOrder: 'sellStock'
            }),
            sellStock() {
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.quantity
                };

                this.placeSellOrder(order);
                this.quantity = 0;
            },
        }
    }
</script>

<style scoped>
  .danger {
      border: 1px solid red;
  }
</style>