<template>
    <div>
        新闻详情 ---- {{ id }}
    </div>
</template>
<script>
export default {
    data() {
        return {
            id: this.$route.params.id, // 将 URL 地址中传递过来的 Id值，挂载到 data上，方便以后调用
            newsinfo: {} // 新闻对象
        };
    }
};
</script>
<style lang="scss">
</style>
