<template>
    <div :class="['header',{'user': user}]">
        <div class="header_icon">
            <img @click="redirectHome" :class="[{'user': !user}]" src="../assets/img/pokeball.png" alt="">
        </div>
        <div v-if="user" class="header_user">
            <img :src="avatar"> <div> {{user}} | <span class="link" @click="logout">sair</span></div>
        </div>
    </div>
</template>
<script>
export default {
   props:['user', 'avatar'],
   methods:{
       logout(){
            localStorage.removeItem('github-user')
            localStorage.removeItem('github-avatar')
            this.$router.push('/login')
       },
       redirectHome(){
           this.$router.push('/')
       }
   }
}
</script>
<style lang="scss">
    .header{
        background: #00f;
        padding: 10px 20px;
        &.user{
            display: flex;
            justify-content: space-between;
        }
        .header_icon{
            img{
                width: 40px;
                height: 40px;
                display: inline-block;
                position: relative;
                &.user{
                    animation: login 5s infinite;
                }
            }
        }
        .header_user{
            color: #fff;
            display: flex;
            align-items: center;
            img{
                width: 35px;
                margin-right: 10px;
                cursor: pointer;
            }
            .link{
                color: #fff;
                text-decoration: underline;
                cursor: pointer;
            }
        }
    }
    @keyframes login {
        0% {
            left: 0px;
            opacity: 1;
        }
        98% {
            left: calc(100% - 40px);
            opacity: 0;
        }
        99% {
            left: 0px;
            opacity: 0;
        }
        100% {
            left: 0px;
            opacity: 1;
        }
    }   
</style>
