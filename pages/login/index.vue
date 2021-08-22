<template>
     <app-frame>
        <div class="container box_container">
            <div class="card_login">
                <h4>Olá, <br> seja bem vindo.</h4>
                <div>
                Para começar, digite seu usuario do github</div>

                <input type="text" v-model="user">
                    <span class="box-error" v-show="githubUser == 'error'">Usuário não identificado</span>
                <button class="botao-primario" :disabled="disabledButton" @click="searchUser">Entrar</button>
            </div>
            
        </div>
    </app-frame>
</template>
<script>
import AppFrame from '@/components/AppFrame.vue';
export default {
    name: 'Login',
    components:{
      AppFrame
    },
    data: function() {
        return {
            user: '',
            githubUser: null
        }
    },
    mounted(){
        
    },
    computed:{
        disabledButton(){
            let disabled = this.user.length == '' 
            return disabled
        }
    },
    methods:{        
        async searchUser() {
            
            try {
                this.githubUser = await this.$axios.$get(`https://api.github.com/users/${this.user}`)

                localStorage.setItem('github-user', this.githubUser.name)
                localStorage.setItem('github-avatar', `https://github.com/${this.githubUser.login}.png`)

                this.$router.push('/')

            } catch (err) {
                localStorage.removeItem('github-user')
                localStorage.removeItem('github-avatar')

                this.githubUser = 'error'
            }
        }
    },
    watch:{
        user(){
            this.githubUser = null
        }
    }
}
</script>
<style lang="scss" scoped>
    .box_container{
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        .card_login{
            width: 250px;
            padding: 15px;
            border: 1px solid #ccc;
            border-radius: 20px;
            height: 350px;
            input{
                border: 1px solid #ccc;
                border-radius: 4px;
                height: 40px;
                width: 100%;
                outline: none;
                padding: 8px 16px;
                color: #30455c;
                font-size: 16px;
                font-weight: 400;
                line-height: 1.5em;
                transition: background-color .2s,border-color .2s,color .2s;
                font-family: "Lato",sans-serif!important;
                margin: 15px auto;

            }
            .botao-primario{
                border-radius: 4px;
                text-align: center;
                display: flex;
                align-items: center;
                justify-content: center;
                min-width: 100px;
                width: 100%;
                cursor: pointer;
                transition: all .2s;
                font-weight: 600;
                letter-spacing: 1px;
                text-transform: uppercase;
                text-decoration: none;
                background: #00f;
                border: none;
                color: #fff;
                height: 40px;
                
            }
            button:disabled{
                cursor: not-allowed;
                background: rgba(#00f, 0.5);
            }
            .box-error{
                    background: #ffffb5;
                    padding: 3px;
                    font-size: 8px;
                    float: right;
                    color: #a5a139;
                    border-radius: 5px;
                    position: relative;
                    top: -8px;                
            }
        }
    }
</style>
