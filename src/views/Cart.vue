<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">장바구니</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th id="thead-title"><h2>상품</h2></th>
                            <th id="thead-title"><h2>가격</h2></th>
                            <th id="thead-title"><h2>수량</h2></th>
                            <th id="thead-title"><h2>합계</h2></th>
                            <th></th>
                        </tr>
                    </thead>

                    <tbody class="contents">
                        <CartItem
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                            v-bind:initialItem="item"
                            v-on:removeFromCart="removeFromCart" />
                    </tbody>
                </table>

                <p v-else>장바구니가 비었습니다 ㅠㅠ...</p>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">상품정보 요약</h2>

                {{ cartTotalLength }}개 상품<br>
                총 가격: <strong>{{ cartTotalPrice }}원</strong>

                <hr>

                <router-link to="/cart/checkout" class="button is-primary is-focused">결제</router-link>
            </div>
        </div>
    </div>
</template>

<script>
import CartItem from '@/components/CartItem.vue'

export default {
    name: 'Cart',
    components: {
        CartItem
    },
    data() {
        return {
            cart: {
                items: []
            }
        }
    },
    mounted() {
        this.cart = this.$store.state.cart
    },
    methods: {
        removeFromCart(item) {
            this.cart.items = this.cart.items.filter(i => i.product.id !== item.product.id)
        }
    },
    computed: {
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
    }
}
</script>

<style lang="scss">
    #thead-title {
        text-align : center;
    }
</style>