<template>
    <div class="page-checkout">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">결제하기</h1>
            </div>

            <div class="column is-12 box">
                <table class="table is-fullwidth">
                    <thead>
                        <tr>
                            <th>상품</th>
                            <th>가격</th>
                            <th>수량</th>
                            <th>합계</th>
                        </tr>
                    </thead>

                    <tbody>
                        <tr
                            v-for="item in cart.items"
                            v-bind:key="item.product.id"
                        >
                            <td>{{ item.product.name }}</td>
                            <td>{{ item.product.price }}원</td>
                            <td>{{ item.quantity }}</td>
                            <td>{{ getItemTotal(item) }}원</td>
                        </tr>
                    </tbody>

                    <tfoot>
                        <tr>
                            <td colspan="2"><h2><strong>총 결제 예정 금액</strong></h2></td>
                            <td><h3>{{ cartTotalLength }}</h3></td>
                            <td><h2>{{ cartTotalPrice }}원</h2></td>
                        </tr>
                    </tfoot>
                </table>
            </div>

            <div class="column is-12 box">
                <h2 class="subtitle">결제 상세정보</h2>

                <p class="has-text-grey mb-4">* 필수 입력 사항</p>

                <div class="columns is-multiline">
                    <div class="column is-6">
                        <div class="field">
                            <label>이름*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="name">
                            </div>
                        </div>


                        <div class="field">
                            <label>이메일*</label>
                            <div class="control">
                                <input type="email" class="input" v-model="email">
                            </div>
                        </div>

                        <div class="field">
                            <label>연락처*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="phone" placeholder="010-0000-0000">
                            </div>
                        </div>
                    </div>

                    <div class="column is-6">
                        <div class="field">
                            <label>배송지*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address">
                            </div>
                        </div>

                        <div class="field">
                            <label>배송지 별명*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="address_label">
                            </div>
                        </div>

                        <div class="field">
                            <label>우편번호*</label>
                            <div class="control">
                                <input type="text" class="input" v-model="zipcode">
                            </div>
                        </div>

                    </div>
                </div>

                <div class="notification is-danger mt-4" v-if="errors.length">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                </div>

                <br>
                
                <div class="columns is-vcentered">
                    <div class="column is-8"></div>
                    <div class="column is-2"></div>
                    <div class="column is-2">
                        <template v-if="cartTotalLength">
                            <input type="image" src="http://127.0.0.1:8000/media/uploads/uploads/kakao_logo_sm.png" height="40" width="97" @click="submitForm">             
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'Checkout',
    data() {
        return {
            cart: {
                items: []
            },
            name: '',
            email: '',
            phone: '',
            address: '',
            address_label: '',
            zipcode: '',
            errors: []
        }
    },
    mounted() {
        document.title = '결제 | Djumper'

        this.cart = this.$store.state.cart

    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        submitForm() {
            this.errors = []
            if (this.name === '') {
                this.errors.push('주문자 본인의 이름을 입력하세요!')
            }
            if (this.email === '') {
                this.errors.push('이메일을 입력하세요!')
            }
            if (this.phone === '') {
                this.errors.push('휴대폰 번호를 입력하세요!')
            }
            if (this.address === '') {
                this.errors.push('배송지를 입력하세요!')
            }
            if (this.address_label === '') {
                this.errors.push('배송지 별명을 입력하세요!')
            }
            if (this.zipcode === '') {
                this.errors.push('우편번호를 입력하세요!')
            }
            if (!this.errors.length) {
                this.$store.commit('setIsLoading', true)
                
                this.kakaoPayReady()
                
                this.$store.commit('setIsLoading', false)
            }
        },
        async kakaoPayReady() {
            const items = []
            for (let i = 0; i < this.cart.items.length; i++) {
                const item = this.cart.items[i]
                const obj = {
                    product: item.product.id,
                    quantity: item.quantity,
                    price: item.product.price * item.quantity
                }
                items.push(obj)
            }
            const data = {
                'name': this.name,
                'email': this.email,
                'address': this.address,
                'address_label': this.address_label,
                'zipcode': this.zipcode,
                'phone': this.phone,
                'items': items,
            }

            this.$store.commit('setIsLoading', true)

            await axios
                .post('/api/v1/checkout/', data)
                .then(response => {
                    this.$store.commit('clearCart')
                    console.log(response)

                    // response의 next_redirect_pc_url로 리다이렉트해서 결제 요청
                    // 결제 성공 시, approval_url + _pg_token_(쿼리스트링) URL로 리다이렉트 --> pg_token 추출
                    // 로딩 중 페이지 새로 만들어서 approval_url로 바꾸고, 최종 결제 승인 response를 받으면 cart/success로 이동하도록 바꾸기
                    let pc_pay_url = response.data.next_redirect_pc_url
                    window.location.href = pc_pay_url
                })
                .catch(error => {
                    this.errors.push('문제가 발생했습니다. 다시 시도해 주세요.')
                    console.log(error)
                    this.$router.push('/')
                })

            this.$store.commit('setIsLoading', false)
        }
    },
    computed: {
        cartTotalPrice() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.product.price * curVal.quantity
            }, 0)
        },
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        }
    }
}
</script>