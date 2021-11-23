<template>
    <div>
        <select v-model="localtablePageSize" @change="changeSelect()" class="pageselect">
            <option v-for="(item, x) in pageSize" :key="x" :value="item.value">{{ item.text }}</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'select-page-size',
    // props: {
    //     tablePageSize
    // },
    data() {
        return {
            localtablePageSize: 0,
            pageSize: [
                {
                    text: '3',
                    value: 3
                },
                {
                    text: '5',
                    value: 5
                },
                {
                    text: '10',
                    value: 10
                },
                {
                    text: 'همه',
                    value: Infinity
                }
            ]
        }
    },
    methods: {
        changeSelect() {
            this.$emit('changePageSize', this.localtablePageSize)
        }
    },
}
</script>