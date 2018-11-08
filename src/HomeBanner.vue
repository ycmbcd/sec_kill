<template>
    <div>
        <div class="flex-wrapper mt10" justify-content="center" v-for="item of list" :key="item.id">
            <div justify-content="center">
                <a :href="item.url">
                	<img :src="item.img" />
                </a>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'HomeBanner',
        props: {
        	list: Array
        }
    }
</script>

<style lang="stylus" scoped>
.banner
    width 100%
img
    width 100%
.box
    margin:0 1%;
.mt10
    margin-top 8px;
</style>
