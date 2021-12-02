<template>
	<div id="home" class="wrapper">
		<nav-bar class="home-nav">
			<div slot="center">购物车</div>
		</nav-bar>
		<tab-control
		ref="tabControl1" 
		:titles="['流行','新款','精选']" 
		@tabClick="tabClick" 
		class="tab-control" v-show="isTabFixed"/>
		
		<scroll class="content" 
						ref="scroll" 
						:probe-type="3" 
						@scroll="contentScroll"
						:pull-up-load="true"
						@pullingUp="loadMore"
						>
			<home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"/>
			<recommend-view :recommends="recommends"></recommend-view>
			<feature-view></feature-view>
			<tab-control 
			ref="tabControl2" 
			:titles="['流行','新款','精选']" 
			@tabClick="tabClick" />
			<goods-list :goods="showGoods"></goods-list>
		</scroll>
		
		<back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
	</div>
</template>

<script>
	import HomeSwiper from './childComps/HomeSwiper.vue';
	import RecommendView from './childComps/RecommendView.vue';
	import FeatureView from './childComps/FeatureView.vue';

	import NavBar from 'components/common/navbar/NavBar.vue';
	import TabControl from '../../components/content/tabControl/TabControl.vue';
	import GoodsList from '../../components/content/goods/GoodsList.vue';
	import Scroll from '../../components/common/scroll/Scroll.vue';
	import BackTop from '../../components/common/backTop/BackTop.vue';

	import {
		getHomeMultidata,
		getHomeGoods
	} from "../../network/home.js";
	// import {Swiper, SwiperItem} from 'components/common/swiper';

import {debounce} from '../../common/utils.js'

	export default {
		name: 'Home',
		components: {
			HomeSwiper,
			RecommendView,
			FeatureView,
			NavBar,
			TabControl,
			GoodsList,
			Scroll,
			BackTop
		},
		data() {
			return {
				banners: [],
				recommends: [],
				goods: {
					'pop': {page: 0,list: []},
					'new': {page: 0,list: []},
					'sell': {page: 0,list: []}
				},
				currentType: 'pop',
				isShowBackTop:false,
				tabOffsetTop:0,
				isTabFixed:false,
				saveY:0
			}
		},
		computed: {
			showGoods() {
				return this.goods[this.currentType].list
			}
		},
		
		// 跳转时回来时,回到跳转前的位置
		activated() {
			this.$refs.scroll.scrollTo(0,this.saveY,0)
			this.$refs.scroll.refresh()
		},
		deactivated() {
			this.saveY = this.$refs.scroll.getScrollY()
			console.log(this.saveY)
		},
		created() {
			// 1.请求多个数据
			this.getHomeMultidata()
			// 2.请求商品数据
			this.getHomeGoods('pop')
			this.getHomeGoods('new')
			this.getHomeGoods('sell')
			
		},
		mounted() {
			
			const refresh = debounce(this.$refs.scroll.refresh,500)
			
			// 3.监听item中图片加载完成
			this.$bus.$on('itemImageLoad',()=>{
				// console.log('图片加载完成')
				// this.$refs.scroll.refresh()
				refresh()
			})
			
			
		},
		methods: {
			// 事件监听相关方法

			tabClick(index) {
				switch (index) {
					case 0:
						this.currentType = 'pop'
						break
					case 1:
						this.currentType = 'new'
						break
					case 2:
						this.currentType = 'sell'
						break
				}
				this.$refs.tabControl1.currentIndex = index;
				this.$refs.tabControl2.currentIndex = index;
			},
			backClick(){
				this.$refs.scroll.scrollTo(0,0,500)
			},
			contentScroll(position){
				// position.y<1000，判断是否显示
				this.isShowBackTop = -position.y>1000
				
				// 2.决定tabControl是否吸顶(position:fixed)
				this.isTabFixed = (-position.y)>this.tabOffsetTop
				
			},
			loadMore(){
				// console.log('上拉加载更多');
				this.getHomeGoods(this.currentType)
				
				// this.$refs.scroll.scroll.refresh()
			},

			swiperImageLoad(){
				// 2.获取tabControl的offsetTop
				this.tabOffsetTop =this.$refs.tabControl2.$el.offsetTop;
			},
			// 网络请求相关的方法
			getHomeMultidata() {
				getHomeMultidata().then(res => {
					// this.result = res;
					this.banners = res.data.banner.list;
					this.recommends = res.data.recommend.list;
				})
			},
			getHomeGoods(type) {
				const page = this.goods[type].page + 1
				getHomeGoods(type, page).then(res => {
					this.goods[type].list.push(...res.data.list);
					this.goods[type].page + 1;
					
					this.$refs.scroll.finishPullUp()
				})
			}
		}
	}
</script>

<style scoped>
	#home {
		/* padding-top: 44px; */
		height: 100vh;
		position: relative;
	}

	.home-nav {
		background-color: var(--color-tint);
		color: #FFFFFF;

	/* 	position: fixed;
		left: 0;
		right: 0;
		top: 0;
		z-index: 9; */
	}


	/* .content{
		height: calc(100%-93px);
		overflow: hidden;
		margin-top: 44px;
	} */
	
	.content{
		/* height: 300px; */
		position: absolute;
		left: 0; 
		right: 0;
		top: 44px;
		bottom: 49px;
		overflow: hidden;
	}
	.tab-control{
		position: relative;
		z-index: 9;
	}
</style>
