<template>
    <div class="card">
        <div class="image">
            <img :src="category.image" alt="">
        </div>
        <div class="name">
            <span>{{category.name}}</span>
        </div>
    </div>
</template>
<script>
export default {
   props:['category']
}
</script>
<style lang="scss" scoped>
	.card{
		display: flex;
		flex-direction: row;
		padding: 15px;
		align-items: baseline;
		transition: background 0.3s;
		.name{
			padding-left: 15px;
			text-transform: uppercase;
			font-weight: bold;
			text-align: center;
			width: 100%;
			transition: .3s;
		}
		img{
			transition: .3s;
		}
		&:hover{
			background: #a4c0ff;
			cursor: pointer;
			.name{
				transform: scale(1.5);
				color:#fff
			}
			img{
				transform: rotate(360deg);
			}
		}
	}
</style>
