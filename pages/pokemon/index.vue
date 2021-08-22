<template>
  <app-frame :user="user" :avatar="avatar">
    

	<div class="container">
		<h2>Olá, sou o pokemon <span class="nome">{{this.nome}}</span></h2>

		<div class="row dados-pokemon">
			<div class="col-6">
				<div v-if="regiao">					
					No universo de Pokémon, fico localizado na regiao <b><a  :href='`https://pokemon.fandom.com/wiki/${regiao.region.name}`' target="_blank"> {{regiao.region.name}}</a></b>, você pode me encontrar lá! 				
				</div>
				<div  v-if="jogos && jogos.version_group" >
					{{jogos.version_group.name.includes('-') ? 'Também pode me contrar nos jogos ' : 'Também pode me encontrar no jogo: ' }}{{jogos.version_group.name.replace('-', ', ')}}
				</div>
				<div v-else>
					Caso não me encontre em seu jogo, poderá me encontrar no anime.
				</div>

				<h6>Me conheça melhor</h6>
				Dados básicos:
				<ul v-if="details">
					<li>Altura: <b>{{details.height/10}} m</b></li>
					<li>Peso: <b>{{details.weight/10}} kg</b></li>
				</ul>

				Dados de luta:
				<ul v-if="details">
					<li :key="index" v-for="(item, index) in details.stats">
						<div>{{stats(item.stat.name)}}: <b>{{item.base_stat}}</b></div>
						<b-progress :max="200">
						<b-progress-bar :value="item.base_stat" show-progress animated :label="`...`"></b-progress-bar>
						</b-progress>
					</li>
				</ul>
				
			</div>
			<div class="col-6">
				<div v-if="details">
					<div class="pokemon">
						<img :src="details.sprites.other.dream_world.front_default" alt="">
					</div>
				</div>
				
			</div>
		</div>
	</div>

	
  </app-frame>
</template>

<script>
import AppFrame from '@/components/AppFrame.vue';
import CardPokemon from '@/components/CardPokemon.vue';

export default {
    name: 'Pokemon',
    components:{
      	AppFrame,
        CardPokemon
    },
    data: () =>{
		return {
			user: null,
			avatar: null,
			list: null,
            nome: null,
            details: null,
            regiao: null,
            jogos: null
		}

    },
    async mounted(){
        let user = localStorage.getItem('github-user');
		
        const route = this.$nuxt.$route;
        const routeQuery = route.query;
        this.nome = routeQuery.nome
       


        if(!user){
			this.$router.push('/login')
		}else{
			this.user = user
        	this.avatar = localStorage.getItem('github-avatar');
			
            await this.basicDetails()
            await this.buscarRegiao() 
            await this.buscarjogos() 
            console.log(this.details)           
            console.log(this.regiao)           
		}
    },
	methods:{
		async basicDetails(){
			const dados = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.nome}/`)
			this.details = dados		
		},
        async buscarRegiao(){
			let dados = null
			try{
				dados = await this.$axios.$get(`https://pokeapi.co/api/v2/location/${this.details.id}/`)
			}catch{
				dados = null
			}
			this.regiao = dados		
		},
        async buscarjogos(){
			let dados = null 
			try{
				dados = await this.$axios.$get(`https://pokeapi.co/api/v2/version/${this.details.id}/`)
			}catch{
				dados = null
			}
			this.jogos = dados		
		},
		stats(item){
			if(item.includes('hp')) item = 'Força'
			if(item.includes('attack')) item = 'Ataque'
			if(item.includes('defense')) item = 'Defesa'
			if(item.includes('special-attack')) item = 'Ataque especial'
			if(item.includes('special-defense')) item = 'Defesa especial'
			if(item.includes('speed')) item = 'Velocidade'
			return item
		},
		detalhes(){
			console.log('detalhes')
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
		.nome{
			color: orange;
		}
	}
	
	.dados-pokemon{
		margin-top: 100px;
		h6{
			margin: 20px 0;
		}
		.pokemon{
			position: relative;
			animation: entradaPokemon 5s linear;
			transition: 2s;
			
			img{
				width: 100%;
			}
		}
	}
}

@keyframes entradaPokemon {
        0% {
            width: 50px;
			left: -100%;
			top: -60px;
        }
        30% {
            width: 50px;
			left: 50%;			
        }
		70% {
            width: 50%;
			left: 30%;
			top: -60px;			
			
        }
        100% {
            width: 100%;
			left: 0;
			top: 0;
        }
    }   
</style>
