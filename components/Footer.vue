<template>
    <div :class="['footer']">
       <span>Desenvolvido por <a href="https://github.com/DiiDenis">@diidenis</a></span>
    </div>
</template>
<script>
export default {
   props:{
      
   }
}
</script>
<style lang="scss">
    .footer{
        background: #00f;
        color:#fff;
        padding: 10px 10px;
        text-align: center;
        a, a:hover{
            color: #fff;
        }
    }
  
</style>
