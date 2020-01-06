<template>
	<view id="menu">
		<!-- 轮播图 -->
		<swiper class="swiper" :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration">
			<swiper-item v-for="(item, index) in bannerList" :key="index">
				<view class="swiper-item">
					<image :src="item.imgUrl" mode="widthFix" />
				</view>
			</swiper-item>
		</swiper>
		<!-- 列表 -->
		<view class="tabbar">
			<scroll-view class="left-tabbar" scroll-y="true">
				<view :class="{'left-item': true, 'active': menuActiveIndex === index, }" v-for="(item, index) in menuList" :key="index" @tap="chooseTitleAction(index)">
					<text>{{item.title}}</text>
				</view>
			</scroll-view>
			<view class="right-tabbar">
				<scroll-view scroll-y="true" class="rightScroll" @scroll="rightScrollAction" :scroll-into-view="rightScrollToId">
					<view class="category-item" v-for="(cateItem, cateIndex) in menuList" :key="cateIndex" :id="(`category-item-${cateIndex}`)">
						<view class="category-title">
							<text>{{cateItem.title}}</text>
						</view>
						<view class="good-list">
							<view class="good-item" v-for="(goodItem, goodIndex) in cateItem.goodList" :key="goodIndex">
								<view class="good-item-left">
									<image :src="goodItem.imgUrl"/>
								</view>
								<view class="good-item-right">
									<view class="item-title">
										<text>{{goodItem.title}}</text>
									</view>
									<view class="item-desc">
										<text>{{goodItem.desc}}</text>
										<text>{{goodItem.defaultChoose}}</text>
									</view>
									<view class="priceWrap">
										<text class="price">{{goodItem.currentPrice}}</text>
										<uni-icons type="plus-filled" size="20" color="#88AFD5"></uni-icons>
									</view>
								</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniIcons from "../../components/uni-icons/uni-icons.vue"
	export default {
		data() {
			return {
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				// 菜单活动 index
				menuActiveIndex: 0,
				// 右边列表 滚动到某个 id
				rightScrollToId: 'category-item-0',
				// 轮播图列表
				bannerList: [],
				// 菜单列表
				menuList: []
			}
		},
		components: {
			uniIcons,
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
			// 点击菜单,滚动
			chooseTitleAction(index) {
				this.menuActiveIndex = index;
				this.rightScrollToId = `category-item-${index}`;
			},
			// 右边滚动
			rightScrollAction(ev) {
				// console.log(ev);
			},
			// 请求数据
			getMenuData() {
				wx.cloud.callFunction({
					name: 'menu-get-data'
				})
				.then(({result}) => {
					let { bannerList, menuList} = result.data[0];
					this.bannerList = bannerList;
					this.menuList = menuList; 
				})
				.catch(err => {
					console.log(err)
					uni.showToast({
						title: '网络繁忙，请稍后再试',
						duration: 2000
					})
				})
			}
		},
		// 页面加载,请求数据
		onLoad() {
			this.getMenuData()
		}
	}
</script>

<style lang="scss" scoped>
	$mainColor: #88AFD5;
	$grayColor: #A6A6A6;
	#menu {
		width: 100%;
		height: 100%;
		display: flex;
		overflow: hidden;
		flex-direction: column;
		.swiper {
			width: 100%;
			background-color: #F8F8F8;
			.swiper-item {
				width: 100%;
				image {
					width: 100%;
				}
			}
		}
		.tabbar {
			width: 100%;
			flex: 1;
			display: flex;
			flex-direction: row;
			background-color: pink;
			overflow: hidden;
			
			.left-tabbar {
				width: 100px;
				height: 100%;
				overflow: hidden;
				background-color: #F8F8F8;
				
				.left-item {
					width: 100%;
					height: 40px;
					line-height: 40px;
					text-align: center;
					font-size: 14px;
					background-color: #F8F8F8;
					color: #505050;
					
					&.active {
						background-color: #FFFFFF;
						color: #383838;
						font-weight: bold;
					}
				}
			}
			.right-tabbar {
				width: 100%;
				height: 100%;
				overflow: hidden;
				flex: 1;
				box-sizing: border-box;
				padding: 0 16px;
				background-color: #fff;
				
				.rightScroll {
					width: 100%;
					height: 100%;
					overflow: hidden;
					
					.category-item {
						.category-title {
							width: 100%;
							height: 40px;
							line-height: 40px;
							text {
								font-size: 12px;
								color: #505050;
								font-weight: bold;
							}
						}
						.good-list {
							.good-item {
								display: flex;
								flex-direction: row;
								align-items: center;
								padding: 12px 0;
								border-bottom: 1px solid #eee;
								
								.good-item-left {
									width: 70px;
									height: 70px;
									flex-shrink: 0;
									border-radius: 4px;
									margin-right: 10px;
									
									image {
										width: 100%;
										height: 100%;
										border-radius: 4px;
									}
								}
								.good-item-right {
									width: 100%;
									
									.item-title {
										font-weight: bold;
										font-size: 14px;
										color: #505050;
									}
									.item-desc {
										display: flex;
										flex-direction: column;
										text {
											font-size: 10px;
											color: $grayColor;
										}
									}
									.priceWrap {
										width: 100%;
										margin-top: 4px;
										display: flex;
										flex-direction: row;
										justify-content: space-between;
										
										.price {
											
										}
									}
								}
							}
						}
					}
				}
			}
		}
	}
</style>
