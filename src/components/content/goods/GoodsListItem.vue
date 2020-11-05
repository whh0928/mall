<template>
	<div class="goods-item" @click="itemClick">
		<img :src="goodsItem.show.img" alt="" @load="imageLoad">
		<div class="goods-info">
			<p>{{goodsItem.title}}</p>
			<span class="goods-price">{{goodsItem.price}}</span>
			<span class="goods-star">{{goodsItem.cfav}}</span>
		</div>
	</div>
</template>

<script>
export default {
  name: 'GoodsListItem',
  props:{
  	goodsItem:{
  		type:Object,
  		default(){
  			return {}
  		}
  	}
  },
  methods:{
  	imageLoad(){
  		this.$bus.$emit('itemImgLoad')
  	},
  	itemClick(){
  		this.$router.push("/detail/"+this.goodsItem.iid)
  	}
  }
}
</script>

<style scoped>
.goods-item {
	width: 48%;
  /*padding-bottom: 40px;*/
}
.goods-item img {
	width: 100%;
	border-radius: 5px;
}
.goods-info {
  font-size: 12px;
  overflow: hidden;
  text-align: center;
  padding: 8px 0;
}
.goods-info p {
	overflow: hidden;
	text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 3px;
}
.goods-price {
	color: var(--color-high-text);
	margin-right: 25px;
}
.goods-star {
	position: relative;
}
.goods-star::before {
	content: '';
	position: absolute;
	left: -15px;
	top: -1px;
	width: 14px;
	height: 14px;
	background: url('~assets/img/common/collect.svg') 0 0/14px 14px;
}
</style>