<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <p class="has-text-grey" style="text-align:center;">잠시 후 결제가 완료됩니다.</p><br>
                <!-- <input type="button" style="text-align:center;" height="40" width="80" @click="paymentAuthorization" value="결제 완료"> -->
                <br>
                <!-- <div class="columns is-vcentered">
                    <div class="column is-4"></div>
                    <div class="column is-4">
                        <button class="button is-primary" style="text-align:center;" @click="paymentAuthorization">결제 완료</button>
                    </div>
                    <div class="column is-4"></div>
                </div> -->


                <div class="notification is-danger mt-4" v-if="errors.length">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Confirm',
    data() {
        return {
            errors: [],
            token: '',
        }
    },
    mounted() {
        document.title = '결제 진행 중.. | Djumper'
        let uri = window.location.search.substring(1)
        let params = new URLSearchParams(uri)
        if (params.has('pg_token')) {
            this.token = params.get('pg_token')
            this.paymentAuthorization()
        }
    },
    methods: {
        async paymentAuthorization() {

            const data = {
                'pg_token': this.token,
                'payment_status': true,
            }

            this.$store.commit('setIsLoading', true)

            await axios
                .patch('/api/v1/confirm/', data)
                .then(response => {
                    console.log(response)
                    this.$router.push('/cart/success')
                })
                .catch(error => {
                    this.errors.push('문제가 발생했습니다. 다시 시도해 주세요.')
                    console.log(error)
                    alert("문제가 발생하여 결제를 취소합니다.")
                    this.$router.push('/')
                })


            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>

<style lang="scss">

</style>