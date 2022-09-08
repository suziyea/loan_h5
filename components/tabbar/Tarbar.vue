<template>
	<view class="container u-flex u-flex-items-center u-flex-around ">
		<view class="tab u-flex u-flex-column u-flex-center  u-flex-items-center " v-for="(item,index) in tabbarList"
			:key="index" @tap="navigatorTo(item)">
			<image v-if="item.page === currentPage" :src="item.selectedIconPath" mode=""></image>
			<image v-else :src="item.iconPath" mode=""></image>
			<view class="text u-flex u-flex-center" :class="{'tabbarActive': currentPage === item.page}">{{item.text}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			currentPage: {
				type: String,
				default: 'index',
			}
		},
		data() {
			return {
				tabbarList: [{
						"pagePath": "/pages/index/index",
						"page": "index",
						"text": "首页",
						"iconPath": "/static/tabIcon/home.png",
						"selectedIconPath": "/static/tabIcon/home_select.png"
					},
					{
						"pagePath": "/pages/mine/mine",
						"page": "mine",
						"text": "我的",
						"iconPath": "/static/tabIcon/center.png",
						"selectedIconPath": "/static/tabIcon/center_select.png"
					}
				]
			}
		},
		onShow() {
			uni.hideTabBar({
				animation: false
			})
		},
		methods: {
			navigatorTo(item) {
				console.log(item, '点击了')
				uni.redirectTo({
					url: `${item.pagePath}`
				});
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 100%;
		position: fixed;
		left: 0;
		bottom: 0;
		height: 98rpx;
		background: #171D41;

		.tab {
			image {
				width: 48rpx;
				height: 48rpx;
			}

			.text {
				font-size: 20rpx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: #B4BCCF;
				line-height: 28rpx;
			}

			.tabbarActive {
				color: #ECC6A1 !important;
			}
		}
	}
</style>
