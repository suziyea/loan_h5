<template>
	<view class="container_prod">
		<u-list @scrolltolower="scrolltolower">
			<view class="swiperBox">
				<u-swiper :list="bannerList" keyName="url" :autoplay="true" bgColor="" @click="swiperClick"
					imgMode="aspectFill"></u-swiper>
			</view>
			<view class="products u-flex u-flex-column u-flex-center">
				<view class="title_view">
					<text class="title">产品</text>
				</view>


				<!-- <view v-if="productList.length > 0">
					<u-list-item v-for="(item,i) in productList" :key="i">
						<view class="productList" @click="clickItem(item)">
							<view class="product-item">
								<view class="logoview" v-if="item.logo">
									<image :src="item.logo" mode="aspectFill"></image>
								</view>
								<view class="left">
									<text class="top_title u-line-1">{{item.name}}</text>
									<text class="mid_title u-line-1">{{item.amount}}</text>
									<text class="bottom_title u-line-1">额度（元）</text>
								</view>
								<view class="center">
									<text class="top_title u-line-1">{{item.remark}}</text>
								</view>
								<view class="right">
									<u-button :plain="true" class="custom-style" :hairline="true" text="一键申请">
									</u-button>
								</view>
							</view>
						</view>
					</u-list-item>
				</view> -->
				<view>

					<view class="product_all u-flex u-flex-center  u-flex-column">
						<u-list @scrolltolower="scrolltolower">

							<u-list-item v-for="(item,index) in productList" :key="index">
								<view class="product_item" @click="clickItem(item)">
									<view class="product_info u-flex u-flex-items-center">
										<view class="logoview">
											<image :src="item.logo" mode="aspectFill"></image>
										</view>
										<view class="product_name">
											{{item.name}}
										</view>
										<view class="tag">
											你好呀
										</view>

									</view>
									<u-divider :hairline="true"></u-divider>
									<view class="product_amount u-flex u-flex-items-center">
										<view class="amount u-flex u-flex-column">
											<view class="money">
												<view class="top_title u-line-1">
													{{item.amount}}
													<text class="mid_title">
														万
													</text>
												</view>
											</view>
											<view class="tips">
												<text class="bottom_title u-line-1">额度（元）</text>
											</view>
										</view>

										<view class="center u-flex u-flex-center">
											<text class="top_title u-line-1">{{item.remark}}</text>
										</view>
										<view class="right_btn u-flex u-flex-center u-flex-items-center">
											一键申请
										</view>

									</view>
								</view>
							</u-list-item>
						</u-list>
					</view>

				</view>



			</view>
		</u-list>

	</view>
</template>

<script>
	import {
		getProducts,
		getBanner,
		clickProductItem
	} from "@/config/api/product.js";
	export default {
		props: {
			randomNum: {
				type: String,
				default: ''
			}
		},
		data() {
			return {
				productList: [],
				pageSize: 10,
				page: 1,
				bannerList: ['/static/img/banner.png', ]
			}
		},
		onLoad() {
			this.loadmore()
		},
		watch: {
			randomNum(newVal, oldVal) {
				this.productList = []
				this.init();
			}
		},
		created() {
			this.init();
		},
		methods: {
			init() {
				this.page = 1;
				this.getEdus();
				this.getBannerImg()
			},
			clickItem(item) {
				uni.$u.debounce(() => this.handleClickProduct(item), 300)
			},
			handleClickProduct(item) {
				clickProductItem({
					id: item.id
				}).then((res) => {
					if (res.code === 100000) {
						uni.navigateTo({
							url: `/pages/webview/webview?urlPath=${encodeURIComponent(item.link)}`
						});
					}
				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			swiperClick(i) {
				let url = this.bannerList[i]?.link || ''
				uni.navigateTo({
					url: `/pages/webview/webview?urlPath=${encodeURIComponent(url)}`
				});
			},
			getBannerImg() {
				getBanner({}).then((res) => {
					if (res.code === 100000) {
						this.bannerList = res?.data?.list || []
					}
				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			getEdus() {
				getProducts({
					"limit": this.pageSize,
					"page": this.page
				}).then((res) => {
					if (res.code === 100000) {
						this.productList.push(...res?.data?.list)
					}
				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			scrolltolower() {
				this.loadmore()
			},
			loadmore() {
				this.page += 1
				this.getEdus()
			}
		},
	}
</script>

<style lang="scss" scoped>
	.container_prod {
		width: 100%;
		// min-height: 100vh;
		background: #F7F7F7;
		padding: 0 40rpx;
		box-sizing: border-box;

		.swiperBox {
			width: 688rpx;
			height: 172rpx;
			margin: 12rpx 0;

			.swiperPage {
				width: 100%;
				height: 100%;
			}
		}

		.products {
			box-sizing: border-box;
			margin-top: 58rpx;

			.titlebox {
				display: flex;
				align-items: center;

				.title {
					font-size: 36rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #333333;
					line-height: 50rpx;
				}

				.icon {
					display: flex;
					margin-left: auto;

					.right_title {
						font-size: 24rpx;
						font-family: PingFangSC-Regular, PingFang SC;
						font-weight: 400;
						color: #666666;
						line-height: 34rpx;
					}

					image {
						width: 32rpx;
						height: 32rpx;
					}
				}
			}


			.product_all {
				width: 100%;
				box-sizing: border-box;

				.product_item {
					width: 670rpx;
					height: 236rpx;
					padding: 0 12px;
					box-sizing: border-box;
					background: #FDFCFA;
					margin-bottom: 36rpx;

					.product_info {
						height: 100%;
						height: 98rpx;

						.logoview {
							image {
								width: 50rpx;
								height: 50rpx;
								border-radius: 6rpx;
							}
						}

						.product_name {
							margin: 12rpx;
							width: 204rpx;
							font-size: 28rpx;
							font-family: PingFangSC-Medium, PingFang SC;
							font-weight: 500;
							color: #363432;
							line-height: 40rpx;
						}

						.tag {
							background: #F7E8DB;
							border-radius: 6rpx;
							font-size: 20rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #B5804F;
							line-height: 28rpx;
							padding: 0 12rpx;
						}
					}

					/deep/.u-divider {
						margin: 0;
					}

					.product_amount {
						height: 100%;
						height: 136rpx;

						// border: 1px solid skyblue;
						.amount {
							// width: 20rpx;
							margin-right: auto;

							.money {
								margin-bottom: 4rpx;

								.top_title {
									// width: 72px;
									// height: 28px;
									font-size: 48rpx;
									font-family: DINAlternate-Bold, DINAlternate;
									font-weight: bold;
									color: #B5804F;
									line-height: 56rpx;
								}

								.mid_title {

									font-size: 28rpx;
									font-family: PingFangSC-Semibold, PingFang SC;
									font-weight: 600;
									color: #B5804F;
									line-height: 40rpx;
									margin-left: 8rpx;
								}
							}

							.tips {
								font-size: 20rpx;
								font-family: PingFangSC-Regular, PingFang SC;
								font-weight: 400;
								color: #9C9B98;
								line-height: 28rpx;
							}
						}

						.center {
							width: 224rpx;

							.top_title {
								height: 34rpx;
								font-size: 24rpx;
								font-family: PingFangSC-Medium, PingFang SC;
								font-weight: 500;
								color: #060503;
								line-height: 34rpx;
							}
						}

						.right_btn {
							width: 156rpx;
							height: 56rpx;
							background: linear-gradient(180deg, #EDDFC5 0%, #CCAC83 100%);
							border-radius: 32rpx;
							margin-left: auto;

							font-size: 26rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #341C0B;
							line-height: 36rpx;
						}
					}
				}
			}

			.productList {
				.product-item {
					box-sizing: border-box;
					width: 100%;
					height: 176rpx;
					background: #FFFFFF;
					box-shadow: 0rpx 10rpx 12rpx 0rpx rgba(0, 0, 0, 0.04);
					border-radius: 8rpx;
					display: flex;
					justify-content: center;
					align-items: center;
					margin-top: 24rpx;

					.logoview {
						margin-left: 24rpx;

						image {
							width: 72rpx;
							height: 72rpx;
						}
					}

					.left {
						display: flex;
						width: 144rpx;
						flex-direction: column;
						justify-content: center;
						height: 100%;
						margin: 0 20rpx;

						.top_title {
							font-size: 24rpx;
							font-family: PingFangSC-Semibold, PingFang SC;
							font-weight: 600;
							color: #282626;
							line-height: 34rpx;
						}

						.mid_title {
							font-size: 36rpx;
							font-family: DINAlternate-Bold, DINAlternate;
							font-weight: bold;
							color: #F0384A;
							line-height: 42rpx;
							margin: 12rpx 0;
						}

						.centermid_title {
							height: 17px;
							font-size: 12px;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #282626;
							line-height: 17px;
						}

						.bottom_title {
							font-size: 24rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #666666;
							line-height: 34rpx;
						}
					}

					.center {
						width: 212rpx;
						display: flex;
						flex-direction: column;
						justify-content: center;
						margin-top: 10rpx;

						.top_title {
							font-size: 20rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #FF9C4A;
							line-height: 28rpx;
						}

						.mid_title {
							font-size: 24rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #282626;
							padding: 12rpx 0;
							line-height: 34rpx;
						}

						.tags {
							display: flex;
							flex-direction: row;
							justify-content: center;
							align-items: center;

							.tagItem {
								display: flex;
								justify-content: center;
								align-items: center;
								width: 88rpx;
								height: 38rpx;
								background: #FFE6CF;
								border-radius: 8rpx;
								border: 2rpx solid #FFC794;
								font-size: 24rpx;
								font-family: PingFangSC-Regular, PingFang SC;
								font-weight: 400;
								color: #FFA34F;
								margin-right: 24rpx;
							}
						}
					}

					.right {
						margin-left: auto;
						margin-right: 28rpx;

						.custom-style {
							width: 152rpx;
							height: 50rpx;
							background: #FF9C4A;
							border-radius: 30rpx;
							font-size: 24rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #FFFFFF;
							line-height: 34rpx;
						}
					}

				}
			}

			&:last-child {
				margin-bottom: 24rpx;
			}
		}

	}

	/deep/ .u-swiper {
		height: 200rpx !important;
	}
</style>
