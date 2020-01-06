<template>
	<view id="home">
		<!-- 轮播图 -->
		<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item, index) in bannerList" :key="index">
				<view class="swiper-item">
					<image :src="item" mode="widthFix" />
				</view>
			</swiper-item>
		</swiper>
		<view class="order-list">
			<order-item :title="recentAddress.shopName" :desc="(`距您${recentAddress.shopDistance}m`)">
				<view class="chooseWay">
					<view :class="{'selfMention': true, 'active': chooseWay}" @tap="chooseWayAction">自提</view>
					<view :class="{'delivery': true, 'active': !chooseWay}" @tap="chooseWayAction">外送</view>
				</view>
			</order-item>
			<order-item :title="item.title" :desc="item.desc" v-for="(item, index) in chooseList" :key="index" isTitleWeight>
				<view class="image-wrap">
					<image :src="item.imgUrl" class="order-img" mode="widthFix" />
				</view>
			</order-item>
		</view>
		<view class="home-footer">
			<image :src="homeFooterImgUrl" />
		</view>
	</view>
</template>

<script>
import OrderItem from '../../components/order-item.vue'
export default {
	data() {
		return {
			indicatorDots: true,
			autoplay: true,
			interval: 2000,
			duration: 500,
			chooseWay: true,
			// 底部图片
			homeFooterImgUrl: '',
			// 可选择的列表
			chooseList: [],
			// 轮播图列表
			bannerList: [],
			// 附近的店
			recentAddress: {}	
		}
	},
	components: {
		OrderItem,
	},
	methods: {
		// 轮播图
		changeIndicatorDots(e) {
			this.indicatorDots = !this.indicatorDots
		},
		changeAutoplay(e) {
			this.autoplay = !this.autoplay
		},
		intervalChange(e) {
			this.interval = e.target.value
		},
		durationChange(e) {
			this.duration = e.target.value
		},
		// 选择方式
		chooseWayAction() {
			this.chooseWay = !this.chooseWay
		},
		// 请求数据方法
		getHomeData() {
			wx.cloud.callFunction({
				name: 'home-get-data'
			})
			.then(({result}) => {
				let { bannerList, chooseList, recentAddress, homeFooterImg } = result.data[0];
				this.homeFooterImgUrl = homeFooterImg;
				this.bannerList = bannerList;
				this.chooseList = chooseList;
				this.recentAddress = recentAddress;
			})
			.catch(err => {
				console.log(err);
				uni.showToast({
				    title: '网络繁忙，请稍后再试',
				    duration: 2000
				});
			})
		}
	},
	// 页面加载,请求数据
	onLoad() {
		this.getHomeData()
	},
}
</script>

<style lang="scss" scoped>
	#home {
		.swiper {
			width: 100%;
			.swiper-item {
				width: 100%;
				image {
					width: 100%;
				}
			}
		}
		// order-list
		.order-list {
			width: 100%;
			padding: 0 14px;
			box-sizing: border-box;
			
			// 方式
			.chooseWay {
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: center;
				width: 94px;
				height: 36px;
				border: 1px solid #88AFD5;
				color: #88AFD5;
				border-radius: 18px;
				box-sizing: border-box;
				padding: 2px;
				
				.selfMention, .delivery {
					flex: 1;
					display: flex;
					justify-content: center;
					align-items: center;
					height: 100%;
					font-size: 12px;
					border-radius: 18px;
					
					&.active {
						background-color: #88AFD5;
						color: #fff;
					}
				}
			}
			
			// 图片
			.image-wrap {
				width: 36px;
				height: 36px;
				display: flex;
				justify-content: center;
				align-items: center;
				border-radius: 50%;
				border: 1px solid rgba(99, 71, 58, 1);
				
				.order-img {
					width: 20px;
					height: 20px;
				}
			}
			
		}
		.home-footer {
			width: 100%;
			height: 54px;
			box-sizing: border-box;
			padding: 2px 30px;
			
			image {
				width: 100%;
				height: 100%;
			}
		}
	}

</style>
