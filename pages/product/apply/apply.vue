<template>
	<view class="container">
		<!-- 背景图片 -->
		<view class="img_bg">
			<view class="copywriting">
				<!-- <view class="bigtitle">
					添加收款银行卡
				</view>
				<view class="desc">
					您的信息将被严格保密，请放心添加
				</view> -->
			</view>
		</view>

		<!-- 产品介绍 -->
		<view class="introduceProductView u-flex u-flex-center u-flex-items-center">
			<view class="introduceContainer u-flex u-flex-column u-flex-center u-flex-items-center">
				<view class="innerTopBg u-flex u-flex-center u-flex-items-center">
					<view class="lineicon">
						<image src="/static/icon/line_icon.png" mode="aspectFill"></image>
					</view>
					<view class="title">
						四大用途
					</view>
					<view class="lineicon">
						<image src="/static/icon/line_icon.png" mode="aspectFill"></image>
					</view>
				</view>
				<view class="innerView">
					<!-- 四大优势 -->
					<view class="usageTips u-flex  u-flex-wrap ">
						<view class="singleUsage u-flex">
							<view class="introduceIcon">
								<image src="../../../static/icon/apply_1.png" mode=""></image>
							</view>
							<view class="introduceDesc">
								<view class="title">
									快速审核
								</view>
								<view class="desc">
									持牌机构专属审核通道，提高下款效率。
								</view>
							</view>
						</view>
						<view class="singleUsage u-flex  ">
							<view class="introduceIcon">
								<image src="../../../static/icon/apply_1.png" mode=""></image>
							</view>
							<view class="introduceDesc">
								<view class="title">
									快速审核
								</view>
								<view class="desc">
									持牌机构专属审核通道，提高下款效率。
								</view>
							</view>
						</view>
						<view class="singleUsage u-flex ">
							<view class="introduceIcon">
								<image src="../../../static/icon/apply_1.png" mode=""></image>
							</view>
							<view class="introduceDesc">
								<view class="title">
									快速审核
								</view>
								<view class="desc">
									持牌机构专属审核通道，提高下款效率。
								</view>
							</view>
						</view>
						<view class="singleUsage u-flex ">
							<view class="introduceIcon">
								<image src="../../../static/icon/apply_1.png" mode=""></image>
							</view>
							<view class="introduceDesc">
								<view class="title">
									快速审核
								</view>
								<view class="desc">
									持牌机构专属审核通道，提高下款效率。
								</view>
							</view>
						</view>
					</view>
					<!-- line -->
					<view class="line"></view>
				</view>
				

			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		methods: {

		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 100%;
		min-height: 100vh;
		background: #090D34;

		.img_bg {
			position: relative;
			width: 750rpx;
			height: 448rpx;
			background: url(../../../static/img/reflect_bg.png) no-repeat;
			background-size: cover;
			// border: 1px solid skyblue;
			box-sizing: border-box;
		}

		.introduceProductView {
			margin-top: -102rpx;
			position: relative;
			border: 1px solid red;
			width: 750rpx;
			height: 732rpx;
			box-sizing: border-box;

			.introduceContainer {
				width: 662rpx;
				height: 732rpx;
				background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				border-radius: 16rpx;
				position: relative;

				.innerView {
					width: 624rpx;
					height: 693rpx;
					border-radius: 16rpx;
					border: 2rpx solid #E5BA71;
					box-sizing: border-box;
					padding: 20rpx;

					// 四大优势
					.usageTips {
						width: 100%;
						height: 272rpx;
						// height: 1000rpx;
						border: 1px solid gold;

						.singleUsage {
							// width: 100%;
							width: 266rpx;
							height: 104rpx;
							margin: 16rpx 0 24rpx 0;

							.introduceIcon {
								margin-left: 22rpx;
								margin-right: 8rpx;

								image {
									width: 56rpx;
									height: 56rpx;
								}
							}

							.introduceDesc {
								// width: 268rpx;
								// width: 250rpx;
								.title {
									// width: 122rpx;
									font-size: 30rpx;
									font-family: PingFangSC-Medium, PingFang SC;
									font-weight: 500;
									color: #B5804F;
									line-height: 42rpx;
								}

								.desc {
									// width: 105px;
									// height: 28px;
									font-size: 20rpx;
									font-family: PingFangSC-Regular, PingFang SC;
									font-weight: 400;
									color: #555A7A;
									line-height: 28rpx;
								}
							}
						}
					}
					.line {
						width: 564rpx;
						height: 2rpx;
						// background: #EDDBC3;
						background: green;
						opacity: 0.29;
					}
				}

				.innerTopBg {
					position: absolute;
					top: -10rpx;
					width: 284rpx;
					height: 56rpx;
					background: url(../../../static/img/apply_content_top.png) no-repeat;
					background-size: cover;

					.lineicon {
						margin: 0 20rpx;

						image {
							width: 16rpx;
							height: 20rpx;
						}
					}

					.title {
						color: #B5804F;
					}
				}
			}

			.introduce {
				width: 662rpx;
				height: 732rpx;
				background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				border-radius: 16rpx;


				.topbg {
					position: absolute;
					// z-index: 5;
					top: 0;
					width: 284rpx;
					height: 56rpx;
					background: url(../../../static/img/apply_content_top.png) no-repeat;
					background-size: cover;

					.lineicon {
						margin: 0 20rpx;

						image {
							width: 16rpx;
							height: 20rpx;
						}
					}

					.title {
						color: #B5804F;
					}
				}
			}
		}
	}
</style>
