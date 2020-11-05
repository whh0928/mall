<template>
	<div class="tab-control">
		<span :class="{active:currentIndex === index}" v-for="(item,index) in titles" @click="itemClick(index)">{{item}}</span>
	</div>
</template>

<script>
export default {
  name: 'TabControl',
  props:{
  	titles:{
  		type:Array,
  		default(){
  			return []
  		}
  	}
  },
  data(){
  	return {
  		currentIndex:0,
  	}
  },
  methods:{
  	itemClick(index){
  		this.currentIndex = index;
  		this.$emit('tabClick',index);
  	}
  }
}
</script>

<style scoped>
.tab-control {
	display: flex;
	text-align: center;
	font-size: 15px;
	padding-bottom: 10px;
	background-color: #fff;
  width: 100%;
}
.tab-control span {
	flex: 1;
	margin: 0 45px;
  height: 30px;
  line-height: 30px;
}
.active {
	color: var(--color-high-text);
	border-bottom: 2px solid var(--color-high-text);
}
</style>