<template>
    <div class="page-success">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">주문이 정상적으로 접수되었습니다.</h1>

                <p>오후 3시 이전 주문 건은 당일 발송되며, 이후에는 다음 날 발송됩니다.(주말, 공휴일 제외)</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Success',
    mounted() {
        document.title = '주문완료 | Djumper'
    },
}
</script>