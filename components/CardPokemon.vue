<template>
    <div class="card">   
		<img :src="image"  alt="">     
        <div class="name">
            <span>{{pokemon.pokemon.name}}</span>
        </div>
		<button class="botao-primario">saber +</button>
    </div>
</template>
<script>
export default {
   props:['pokemon'],
   computed:{
	   numero(){
		    let numero = this.pokemon.pokemon.url.split('/')[6]
		    if(numero.length == 1) 
				numero = '00' + numero 
			
			if(numero.length == 2) 
				numero = '0' + numero 

			return numero
	   },
	   image(){
		   return `https://assets.pokemon.com/assets/cms2/img/pokedex/detail/${this.numero}.png`
	   }
   },
   methods:{
	   detalhesPokemon(){
		   console.log('treste')
	   }
   }
}
</script>
<style lang="scss" scoped>
	.card{		
		padding: 15px;		
		transition: background 0.3s;
		.name{
			padding-left: 15px;
			text-transform: uppercase;
			font-weight: bold;
			text-align: center;
			width: 100%;
			transition: .3s;
			margin-bottom: 15px;
		}		
		&:hover{
			background: #ffb37d;
			cursor: pointer;
			.name{
				transform: scale(1.5);
				color:#fff
			}
			.botao-primario{
				background:#00f
			}			
		}
	}
	.botao-primario{
		border-radius: 4px;
		text-align: center;
		display: flex;
		align-items: center;
		justify-content: center;
		min-width: 100px;
		width: 50%;
		cursor: pointer;
		transition: all .2s;
		font-weight: 600;
		letter-spacing: 1px;
		text-transform: uppercase;
		text-decoration: none;
		background: #ccc;
		border: none;
		color: #fff;
		height: 40px;
		margin: 0 auto;
		
	}
	button:disabled{
		cursor: not-allowed;
		background: rgba(#00f, 0.5);
	}
</style>
