<template>
	<nav-bar>
		<div slot="left" class="back"><img src="~assets/img/common/back.svg" alt=""></div>
		<div slot='center' class="titles">
			<div v-for="(item,index) in titles" @click="titleClick(index)" :class="{active:currentIndex===index}">{{item}}</div>
		</div>
	</nav-bar>
</template>

<script>
	import NavBar from 'components/common/navbar/NavBar'

export default {
  name: 'DetailNavBar',
  components:{
  	NavBar,
  },
  data(){
  	return {
  		titles:['商品','参数','评论','推荐'],
  		currentIndex:0,
  	}
  },
  computed:{
  	activeColor(){
  		return this.currentIndex === index ? {color:"var(--color-high-text)"} : {}
  	}
  },
  methods:{
  	titleClick(index){
  		this.currentIndex = index;
  	}
  }
}
</script>

<style scoped>
.back img {
	width: 22px;
	vertical-align: middle;
}
.titles {
	display: flex;
}
.titles div {
	flex: 1;
	font-size: 14px;
}
.active {
	color: var(--color-high-text)
}
</style>