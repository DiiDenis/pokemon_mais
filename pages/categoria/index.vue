<template>
  <app-frame :user="user" :avatar="avatar">
    

	<div class="container">
		<h2>Selecione um Pokemon</h2>
		<div class="row">
			<div class="col-4 col-xs-1 mb15" :key="index" v-for="(item, index) in list">
				<card-pokemon @click.native="detalhes(item)" :pokemon="item"/>
			</div>
		</div>      
		
	</div>

	
  </app-frame>
</template>

<script>
import AppFrame from '@/components/AppFrame.vue';
import CardPokemon from '@/components/CardPokemon.vue';

export default {
    name: 'Categoria',
    components:{
      	AppFrame,
        CardPokemon
    },
    data: () =>{
		return {
			user: null,
			avatar: null,
			list: null,
            endpoint: null
		}

    },
    async mounted(){
        let user = localStorage.getItem('github-user');
		
        const route = this.$nuxt.$route;
        const routeQuery = route.query;
        this.endpoint = routeQuery.type
       


        if(!user){
			this.$router.push('/login')
		}else{
			this.user = user
        	this.avatar = localStorage.getItem('github-avatar');
			this.listCategory()
		}
    },
	methods:{
		async listCategory(){
			const teste = await this.$axios.$get(`https://pokeapi.co/api/v2/type/${this.endpoint}/`)	
			
			this.list = teste.pokemon.slice(0, 20);			
		},
		detalhes(item){
			this.$nuxt.$router.push(
				{
					path: '/pokemon',
					query: {
						nome: item.pokemon.name
					}
				}
			);
		}		
	}
}
</script>
<style lang="scss" scoped>
.container{
	padding: 15px;
	h2{
		text-align: center;
		margin-bottom: 20px;
	}
	.mb15{
		margin-bottom: 15px;
	}
}
</style>
