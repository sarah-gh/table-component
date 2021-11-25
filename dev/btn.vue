<template>
    <div>
        <button>{{ textBtn }}</button>
    </div>
</template>
<script>
export default {
    name: 'my-btn',
    props: {
        textBtn: {
            typeof: String,
            default: 'ttt'
        }
    }
}
</script>