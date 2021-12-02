<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"/>
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
			<detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
			<detail-param-info :param-info="paramInfo"/>
			<detail-comment-info :commentInfo="commentInfo" ref="goodsComment"/>
		</scroll>
  </div>
</template>

<script>
	import DetailNavBar from './childComps/DetailNavBar.vue'
	import DetailSwiper from './childComps/DetailSwiper.vue'
	import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
	import DetailShopInfo from './childComps/DetailShopInfo.vue'
	import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
	import DetailParamInfo from './childComps/DetailParamInfo.vue'
	import DetailCommentInfo from './childComps/DetailCommentInfo';

	import Scroll from '../../components/common/scroll/Scroll.vue'

	import {
		getDetail,
		Goods,
		Shop,
		GoodsParam
	} from "../../network/detail.js"


	export default {
		name: "Detail",
		components: {
			DetailNavBar,
			DetailSwiper,
			DetailBaseInfo,
			DetailShopInfo,
			DetailGoodsInfo,
			DetailParamInfo,
			DetailCommentInfo,
			Scroll
		},
		data() {
			return {
				iid: null,
				topImages: [],
				goods: {},
				shop: {},
				detailInfo: {},
				paramInfo: {},
				commentInfo:{}
			}
		},
		created() {
			// 1.保存传入的id
			this.iid = this.$route.params.iid
			// 2.根据iid请求详细数据
			getDetail(this.iid).then(res => {
				// 1.获取顶部的图片轮播数据
				console.log(res);
				const data = res.result;
				this.topImages = data.itemInfo.topImages

				// 2.获取商品信息
				this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

				// 3.创建店铺信息
				this.shop = new Shop(data.shopInfo)
				// 4.保存商品的详细信息
				this.detailInfo = data.detailInfo
				// 5.获取参数信息
				this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
				
				this.commentInfo = data.rate;
			})
		},
		methods: {
		  imageLoad() {
				
				
		    this.$refs.scroll.refresh()
		  }
		}
	}
</script>

<style scoped>
  #detail {
    height: 100vh;
    position: relative;
    background-color: #ffffff;
    /* height: 100vh; */
  }

  .detail-nav {
    /* position: relative;
    z-index: 9; */
    background-color: #ffffff;
  }

  .content {
    position: absolute;
    left: 0; 
    right: 0;
    top: 44px;
    bottom: 49px;
    overflow: hidden;
  }
</style>
