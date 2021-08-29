<template>
    <tr>
        <td id="con">
            <div class="img-size">
                <router-link :to="item.product.get_absolute_url">
                    <img class="img-item" v-bind:src="item.product.get_thumbnail">
                </router-link>
            </div><br>
            <span style="text-align: center;">
                {{ item.product.name }}
            </span>
            
        </td>
        <td id="con">
            {{ item.product.price }}
        </td>
        <td id="con">
            {{ item.quantity }}
            <a class="tag is-primary is-light is-medium" @click="decrementQuantity(item)"><strong>-</strong></a>
            <a class="tag is-primary is-light is-medium" @click="incrementQuantity(item)"><strong>+</strong></a>
        </td>
        <td id="con">
            {{ getItemTotal(item) }}      
        </td>
        <td id="con">
            <button class="button is-danger is-outlined" @click="removeFromCart(item)">
                <span class="icon is-small">
                    <i class="fas fa-times"></i>
                </span>
            </button>
        </td>
    </tr>
</template>

<script>
export default {
    name: 'CartItem',
    props: {
        initialItem: Object
    },
    data() {
        return {
            item: this.initialItem
        }
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        decrementQuantity(item) {
            item.quantity -= 1
            if (item.quantity === 0) {
                this.$emit('removeFromCart', item)
            }
            this.updateCart()
        },
        incrementQuantity(item) {
            item.quantity += 1
            this.updateCart()
        },
        updateCart() {
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        removeFromCart(item) {
            this.$emit('removeFromCart', item)
            this.updateCart()
        },
    },
}
</script>

<style lang="scss">
    .img-size {
        width: 128px;
        height: 128px; 
        border-radius: 20%;
        overflow: hidden;
        margin: auto;
    }
    .img-item {
        width: 100%;
        height: 100%;
        object-fit: cover;
        display:block; 
        margin:auto;
    }
    #con {
        text-align : center;
        vertical-align : middle;
    }
</style>