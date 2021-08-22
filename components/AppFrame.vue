<template>
    <div class="app-frame">
        <Header :user="user" :avatar="avatar" />
        <div  class="content">
            <slot></slot>
        </div>
        <Footer />
    </div>
</template>
<script>
import Header from '@/components/Header.vue';
import Footer from '@/components/Footer.vue';
export default {
    components:{
        Header,
        Footer
    },
    props:['user', 'avatar']
}
</script>
<style lang="scss" scoped>

.app-frame{
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: auto 1fr auto;
    height: 100%;

    .header{
        grid-area:'header'
    }
    .content{
        grid-area:'content'
    }
    .footer{
        grid-area:'footer'
    }
}


</style>
