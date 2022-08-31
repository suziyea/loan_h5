<template>
	<view class="container  u-flex u-flex-column" >
		<!-- <view class="cells">
			<u-cell-group>
				<view class="tel" v-for="(item,index) in telphones" :key="index">
					<u-cell :title="item">
						<u-icon slot="value" name="phone" :index='index' color="#2979ff" size="28" @click="toTel">
						</u-icon>
					</u-cell>
				</view>
			</u-cell-group>
		</view> -->
		
		<view class="page_bg u-flex u-flex-column u-flex-items-center">
			<view class="bg_style">
				<image src="/static/img/contact_bg.png" mode="aspectFill"></image>
			</view>
			<view class="call_tel">
				客服电话:010-81377368
			</view>
		</view>
		
		<view class="btn_view u-flex u-flex-center">
			<view class="callbtn u-flex u-flex-center u-flex-items-center" @click="toTel(1)">
				拨打客服电话
			</view>
		</view>

	</view>
</template>

<script>
	import {
		getEdu
	} from "@/config/api/user.js";
	export default {
		data() {
			return {
				emptyStatus: true,
				telphones: []
			}
		},
		created() {
			this.getEdus()
		},
		methods: {
			getEdus() {
				getEdu({
					"code": "customer_service_phone"
				}).then((res) => {
					if (res.code === 100000) {
						if (res?.data?.value?.value) {
							this.telphones = res?.data?.value?.value.split(',')
							return
						}
						this.telphones = []
					}
				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			toTel(i) {
				uni.makePhoneCall({
					phoneNumber: this.telphones[i]
				});
			}
		},
		computed: {
			addClassName() {
				if (this.emptyStatus) {
					// return 'u-flex u-flex-column u-flex-center u-flex-items-center'
				}
				return ''
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 100%;
		min-height: 100vh;
		.page_bg {
			width: 100%;
			margin-top: 142rpx;
			padding: 0rpx  44rpx;
			box-sizing: border-box;
			.bg_style {
				margin-bottom: 66rpx;
				image {
					width: 332rpx;
					height: 364rpx;
				}
			}
			.call_tel {
				width: 396rpx;
				height: 50rpx;
				font-size: 36rpx;
				font-family: PingFangSC-Medium, PingFang SC;
				font-weight: 500;
				color: #312D2D;
				line-height: 50rpx;
			}
		}
		
		.btn_view {
			width: 100%;
			margin-top: auto;
			margin-bottom: 104rpx;
			.callbtn {
				width: 662rpx;
				height: 88rpx;
				background: linear-gradient(180deg, #EDDFC5 0%, #CCAC83 100%);
				border-radius: 44rpx;
				font-size: 32rpx;
				font-family: PingFangSC-Medium, PingFang SC;
				font-weight: 500;
				color: #341C0B;
				line-height: 44rpx;
			}
		}

		.cells {
			margin: 30rpx 30rpx;

			/deep/ .u-cell-group__wrapper {
				background: #FFFFFF;

			}

			/deep/ .u-cell__body {
				padding: 28rpx 30rpx;
			}
		}
	}
</style>
