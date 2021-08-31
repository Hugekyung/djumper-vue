<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">회원 정보</h1>
            </div>

            <!-- 기존 회원 정보 조회 & 수정 -->
            <div class="column is-12">
                <p class="title">이름</p>
                <p class="title">이메일</p>
                <p class="title">비밀번호</p>
                <p class="title">비밀번호 확인</p>
            </div>

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">로그 아웃</button>
            </div>

            <hr>

            <div class="column is-12">
                <h2 class="subtitle">주문 현황</h2>

                <OrderSummary
                    v-for="order in orders"
                    v-bind:key="order.id"
                    v-bind:order="order" />
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import OrderSummary from '@/components/OrderSummary.vue'

export default {
    name: 'MyAccount',
    components: {
        OrderSummary
    },
    data() {
        return {
            orders: []
        }
    },
    mounted() {
        document.title = '회원정보 | Djumper'
        this.getMyOrders()
    },
    methods: {
        logout() {
            axios.defaults.headers.common["Authorization"] = ""
            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")
            this.$store.commit('removeToken')
            this.$router.push('/')
        },
        async getMyOrders() {
            this.$store.commit('setIsLoading', true)
            await axios
                .get('/api/v1/orders/')
                .then(response => {
                    this.orders = response.data
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>