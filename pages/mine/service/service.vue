<template>
	<view class="container">
		<!-- <view class="service">
			<view class="logo">
				<image src="/static/icon/logo.png" mode="aspectFill"></image>
			</view>
			<view class="cells">
				<u-cell-group>
					<u-cell title="客服电话" icon="/static/icon/tel_icon.png" isLink url="/pages/mine/telPhone/telPhone">
					</u-cell>
					<u-cell title="改善建议/投诉反馈" icon="/static/icon/callback_icon.png" isLink
						url="/pages/mine/feedback/feedback"></u-cell>

					#ifdef APP-PLUS
					<u-cell title="版本号" icon="setting">
						<text slot="value" class="u-slot-value">版本 {{version}}</text>
					</u-cell>
					#endif
				</u-cell-group>
			</view>
		</view> -->

		<view class="servece_view u-flex u-flex-center u-flex-between u-flex-wrap">
			<view class="sigle_container u-flex u-flex-column u-flex-items-center " v-for="(item,index) in cellList" :key="index" @click="handleListItem(item)">
				<view class="bg_style">
					<image :src="item.bgImg" mode="aspectFill"></image>
				</view>
				<view class="title">
					{{item.name}}
				</view>
			</view>
			<!-- <view class="sigle_container">

			</view>
 -->
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				version: '',
				cellList: [{
						bgImg: '/static/img/service_phone.png',
						path: '/pages/mine/telPhone/telPhone',
						name: '客服电话',
						enName: 'look',
						power: false
					},
					{
						bgImg: '/static/img/service_remark.png',
						path: '/pages/mine/feedback/feedback',
						name: '反馈/建议',
						enName: 'about',
						power: false
					},
					// {
					// 	bgImg: '/static/img/service_remark.png',
					// 	path: '',
					// 	name: '注销账户',
					// 	enName: 'logoff'
					// },
					// {
					// 	bgImg: '/static/img/service_phone.png',
					// 	path: '/pages/login/login',
					// 	name: '退出登录',
					// 	enName: 'logout',
					// 	power: true
					// }
				],
			};
		},
		created() {
			// #ifdef APP-PLUS
			this.version = plus.runtime.version;
			// #endif


		},
		methods: {
			handleListItem(item) {
				if (item.path) {
					uni.$u.route(item.path)
					return;
				}
			},
		}

	}
</script>

<style lang="scss" scoped>
	.container {
		width: 750rpx;
		height: 100vh;
		background: #090D34;

		.servece_view {
			padding: 0 60rpx;
			box-sizing: border-box;

			.sigle_container {
				width: 288rpx;
				height: 344rpx;
				background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				border-radius: 16rpx;
				margin-top: 36rpx;
				position: relative;
				.bg_style {
					margin-top: 28rpx;
					image {
						width: 240rpx;
						height: 240rpx;
					}
				}
				.title {
					font-size: 28rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #341C0B;
					line-height: 40rpx;
				}
			}
		}

		.service {
			padding-top: 60upx;

			.logo {
				width: 182upx;
				height: 182upx;
				margin: 0 auto 57upx auto;

				image {
					width: 100%;
					height: 100%;
				}
			}

			/deep/ .u-cell-group__wrapper {
				background: #FFFFFF;

			}

			/deep/ .u-cell__body {
				padding: 28rpx 30rpx;
			}
		}
	}
</style>
