<template>
	<div class="goods-item" @click="itemClick">
		<img :src="goodsItem.show.img" @load="imageLoad">
		<div class="goods-info">
			<p>{{goodsItem.title}}</p>
			<span class="price">{{goodsItem.price}}</span>
			<span  class="collect">{{goodsItem.cfav}}</span>
		</div>
	</div>
</template>

<script>
	export default {
	    name: 'GoodsListItem',
	    components: {
	        
	    },
			props:{
				goodsItem:{
					type:Object,
					default(){
						return{}
					}
				}
			},
			methods:{
				imageLoad(){
					// console.log('图片加载完成')
					this.$bus.$emit('itemImageLoad')
				},
				itemClick(){
					this.$router.push('/detail/' + this.goodsItem.iid)
					
				}
			}
	}
</script>

<style>
	.goods-item {
	    position: relative;
	    padding-bottom: 40px;
	    width: 48%;
	}
	.goods-item img{
	    width: 100%;
	    border-radius: 5px;
	}
	.goods-info {
		  font-size: 14px;
			position: absolute;
	    padding: 3px;
	    bottom: 0px;
	    left:0;
	    right: 0;
	    overflow: hidden;
	    text-align: center;
	}
	.goods-info p{
	    overflow: hidden;
	    text-overflow: ellipsis;
	    white-space: nowrap;
			margin-bottom: 3px;
	}
	.goods-info .price {
	    color: var(--color-high-text);
	    margin-right: 24px;
	}
	.goods-info .collect {
	    position: relative;
	}
	.goods-info .collect::before{
	    content: '';
	    position: absolute;
	    top: 0;
	    left: -16px;
	    width: 14px;
	    height: 14px;
	    background: url('~@/assets/img/common/collect.svg') 0 0/14px 14px;
	}
</style>
