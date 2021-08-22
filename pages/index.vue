<template>
  <app-frame :user="user" :avatar="avatar">
    

	<div class="container">
		<h2>Selecione uma categoria de Pokemon</h2>
		<div class="row">
			<div class="col-4 mb15" :key="item.name" v-for="item in categorias">
				<card-category @click.native="clickedCategoria(item)" :category="item"/>
			</div>
		</div>
		
	</div>

	
  </app-frame>
</template>

<script>
import AppFrame from '@/components/AppFrame.vue';
import CardCategory from '@/components/CardCategory.vue';

export default {
    name: 'Home',
    components:{
      	AppFrame,
        CardCategory
    },
    data: () =>{
		return {
			user: null,
			avatar: null,
			categorias: null,
			listaImagesByCategory: null
		}

    },
    async mounted(){
        let user = localStorage.getItem('github-user');
		


        if(!user){
			this.$router.push('/login')
		}else{
			this.user = user
        	this.avatar = localStorage.getItem('github-avatar');
			await this.listaImageCategory()
			this.listCategory()
		}
    },
	methods:{
		async listCategory(){
			const {results} = await this.$axios.$get(`https://pokeapi.co/api/v2/type/`)	
			results.forEach(item => {
				item.image = this.listaImagesByCategory.filter(image =>
					image.category == item.name ? image.url_image : ''
				)[0].url_image
			});
			this.categorias = results
			
		},
		async listaImageCategory(){
			let {list} = await this.$axios.$get(`${window.location.href}assets/mock/imageByCategory.json`)
			this.listaImagesByCategory = list			
		},
		clickedCategoria(categoria){
			this.$nuxt.$router.push(
				{
					path: '/categoria',
					query: {
						type: categoria.url.split('/')[6]
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
