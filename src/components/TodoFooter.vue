<template>
    <div class="clearAllContainer">
        <span class="clearAllBtn" @click="clearTodo">모두 지우기</span>
    </div>
</template>

<script>
export default {
    methods: {
        clearTodo() {
            this.$emit('removeAll');
        }
    }
}
</script>

<style scoped>
    .clearAllContainer {
        width: 7.0rem;
        height: 40px;
        line-height: 40px;
        background-color: #fffcf0;
        border-radius: 5px;
        margin: 0 auto;
    }
    .clearAllBtn {
        color: #cb7575;
        display: block;
        font-family: 'Poor Story', cursive;
    }
</style>