<template>
	<div class="home-recommend">
		<div class='recommends' v-for='item in recommends'>
			<a :href="item.link">
				<img :src="item.image" alt="">
				<div>{{item.title}}</div>
			</a>
		</div>
	</div>
</template>

<script>
export default {
  name: 'HomeRecommend',
  props:{
  	recommends:{
  		type:Array,
  		default(){
  			return []
  		}
  	}
  },
  data(){
  	return {
  		
  	}
  }
}
</script>

<style scoped>
.home-recommend {
	width: 100%;
	display: flex;
	text-align: center;
	padding: 10px 0 20px 0;
	font-size: 12px;
	border-bottom: 10px solid #eee;
}
.recommends {
	flex: 1;
}
.recommends img {
	width: 70px;
	height: 70px;
	margin-bottom: 10px;
}
</style>