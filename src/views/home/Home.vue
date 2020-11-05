<template>
	<div id="home">
		<nav-bar class='home-nav'>
			<span slot='center'>购物街</span>
		</nav-bar>
		<tab-control class='tab-control1' ref='tabControl1' :titles="['流行','新款','精选']" @tabClick="tabClick" v-show='isTabFixed'></tab-control>

		<scroll class="content" ref="scroll" :pull-up-load="true" :probe-type="3" @pullingUp="loadMore" @scroll="contentScroll">
			<home-swiper :banners='banners' @swiperImageLoad="swiperImageLoad"></home-swiper>
			<home-recommend :recommends='recommends'></home-recommend>
			<feature-view></feature-view>
			<tab-control ref='tabControl2' :titles="['流行','新款','精选']" @tabClick="tabClick"></tab-control>
			<goods-list :goods="showGoods"></goods-list>
		</scroll>

		<back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
	</div>
</template>

<script>
	import NavBar from 'components/common/navbar/NavBar'
	import TabControl from 'components/content/tabControl/TabControl'
	import GoodsList from 'components/content/goods/GoodsList'
	import Scroll from 'components/common/scroll/Scroll'
	import BackTop from 'components/content/backTop/BackTop'
	
	import HomeSwiper from './childComps/HomeSwiper'
	import HomeRecommend from './childComps/HomeRecommend'
	import FeatureView from './childComps/FeatureView'

	import {getHomeMultidata,getHomeGoods} from 'network/home.js'
	import {debounce} from 'common/utils.js'
export default {
  name: 'Home',
  components:{
  	NavBar,
  	HomeSwiper,
  	HomeRecommend,
  	FeatureView,
  	TabControl,
  	GoodsList,
  	Scroll,
  	BackTop,
  },
  data(){
  	return {
  		banners:[],
  		recommends:[],
  		types:['pop','new','sell'],
  		currentType:'pop',
  		goods:{
  			'pop':{ page:1 , list:[] },
  			'new':{ page:1 , list:[] },
  			'sell':{ page:1 , list:[] }
  		},
  		isShowBackTop:false,
  		isTabFixed:false,
  		tabOffsetTop:0,
  		saveY:0,
  		itemImgRefresh:null,
  	}
  },
  created(){
  	this.getHomeMultidata();
  	this.getHomeGoods('pop');
  	this.getHomeGoods('new');
  	this.getHomeGoods('sell');
  },
  mounted(){
  	const refresh = debounce(this.$refs.scroll.refresh,50);
		this.itemImgRefresh = () => {
			refresh();
		}
		this.$bus.$on('itemImgLoad',this.itemImgRefresh)
  },
  activited(){
  	this.$refs.scroll.scrollTo(0,this.saveY,0);
  	this.$refs.scroll.refresh();
  },
  deactivited(){
  	this.saveY = this.$refs.scroll.getScrollY();
  	this.$bus.$off('itemImgLoad',this.itemImgRefresh)
  },
  computed:{
  	showGoods(){
  		return this.goods[this.currentType].list;
  	}
  },
  methods:{
  	getHomeMultidata(){
  		getHomeMultidata().then(res => {
  			this.banners = res.data.banner.list
  			this.recommends = res.data.recommend.list
  		}
  			)
  	},
  	getHomeGoods(type){
  		const page = this.goods[type].page;
  		getHomeGoods(type,page).then(res => {
  			this.goods[type].list.push(...res.data.list);
  			this.goods[type].page += 1;
  			console.log(res.data)

  			this.$refs.scroll.finishPullUp();
  		})
  	},
  	tabClick(index){
  		this.currentType = this.types[index];

  		this.$refs.tabControl1.currentIndex = index;
  		this.$refs.tabControl2.currentIndex = index;
  	},
  	loadMore(){
  		this.getHomeGoods(this.currentType)
  	},
  	backClick(){
  		this.$refs.scroll.scrollTo(0,0,300);
  	},
  	contentScroll(position){
  		this.isShowBackTop = (-position.y) > 1000;

  		this.isTabFixed = (-position.y + 44) > this.tabOffsetTop;
  	},
  	swiperImageLoad(){
  		this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
  	},
  }
}
</script>

<style scoped>
#home {
	position: relative;
	width: 100vw;
	height: 100vh;
}
.home-nav {
	color: #fff;
	background-color: var(--color-tint);
}
.content {
	overflow: hidden;
	height: calc(100% - 44px - 49px);
}
.tab-control1 {
	position: fixed;
	top: 44px;
	z-index: 10;
}
</style>