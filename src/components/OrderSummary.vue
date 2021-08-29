<template>
    <div class="box mb-4">
        <h3 class="is-size-4 mb-6">주문 정보 #{{ order.id }}</h3>

        <h4 class="is-size-5">상품 상세</h4>

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
                    v-for="item in order.items"
                    v-bind:key="item.product.id"
                >
                    <td>{{ item.product.name }}</td>
                    <td>{{ item.product.price }}원</td>
                    <td>{{ item.quantity }}개</td>
                    <td>{{ getItemTotal(item) }}원</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'OrderSummary',
    props: {
        order: Object
    },
    methods: {
        getItemTotal(item) {
            return item.quantity * item.product.price
        },
        orderTotalLength(order) {
            return order.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        },
    }
}
</script>