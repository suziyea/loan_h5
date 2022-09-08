<template>
	<!-- <view class="container" :class="addClassName" v-if="showOrderFlag">
		<view class="empty" v-if="emptyStatus">
			<u-empty mode="order" :text="emptyText" icon="http://cdn.uviewui.com/uview/empty/order.png">
			</u-empty>
		</view>
		<view v-else class="order_list">
			<u-list @scrolltolower="scrolltolower">
				<u-list-item v-for="(item, index) in orderList" :key="index">
					<u-cell-group>
						<u-cell :title="item.order_type | formatOrderType" :value="`¥ ${item.order_amount} 元`"
							:label='item.pay_time | formatGetTime' center>
							<u-tag slot="icon" :text="item.is_paid | formatPayType" plain size="mini"
								:type="item.is_paid | formatTagColorType"></u-tag>

						</u-cell>
					</u-cell-group>
				</u-list-item>
			</u-list>
		</view>
	</view> -->
	<view class="container">
		<!-- <u-sticky bgColor="#fff"> -->
		<u-tabs lineColor="#EDDBC3" lineWidth="80rpx" :scrollable="false" :list="topBarList"
			itemStyle="width:250rpx; height: 88rpx; padding:0rpx;"
			activeStyle="font-size: 28rpx;font-weight: 500;color: #EDDBC3;" @click="click">
		</u-tabs>
		<!-- </u-sticky> -->

		<swiper :current='topBarIndex'>

			<swiper-item v-for="(item,index) in topBarList" :key="index">
				<view class="orderRecord u-flex u-flex-column u-flex-items-center">
					<view class="orderinfo u-flex u-flex-center u-flex-items-center ">
						<view class="name">
							<view class="title">
								第二笔付款
							</view>
							<view class="date">
								2022-08-08 19:23:33
							</view>
						</view>
						<view class="status u-flex u-flex-center u-flex-items-center successBgColor">
							未支付
						</view>
						<view class="money">
							0.02<text class="smallText">元</text>
						</view>
					</view>
					<view class="orderinfo">

					</view>
					<view class="orderinfo">

					</view>
				</view>
			</swiper-item>

		</swiper>
	</view>
</template>

<script>
	import {
		getMyOrderList
	} from "@/config/api/product.js";
	export default {
		data() {
			return {
				emptyText: '暂无订单',
				orderList: [],
				showOrderFlag: false,
				topBarList: [{
					name: '全部',
				}, {
					name: '已支付',
				}, {
					name: '未支付'
				}],
				topBarIndex: ''
			}
		},
		created() {
			this.getInitList()
		},
		onLoad() {
			this.loadmore()
		},
		filters: {
			formatOrderType(value) {
				if (value == '1') {
					return '第一笔付款'
				}
				if (value == '2') {
					return '第二笔付款'
				}
				return ''
			},
			formatPayType(value) {
				if (value == 1) {
					return '已支付'
				}
				if (value == 2) {
					return '未支付'
				}
				return '失败'
			},
			formatTagColorType(value) {
				if (value == 1) {
					return 'success'
				}
				if (value == 2) {
					return 'warning'
				}
				return 'error'
			},
			formatGetTime(val) {
				let timestamp = new Date(val).getTime()
				return uni.$u.timeFormat(timestamp, 'yyyy-mm-dd hh:MM:ss');
			}

		},
		methods: {
			click(item) {
				console.log('item', item);
				this.topBarIndex = item.index;
			},
			getInitList() {
				getMyOrderList({}).then((res) => {
					if (res.code === 100000) {
						if (res?.data?.length > 0) {
							this.orderList = res?.data
						}
					}
				}).catch((err) => {
					console.log(err, 'err');
				}).finally(() => {
					this.showOrderFlag = true;
				})
			},
			scrolltolower() {
				this.loadmore()
			},
			loadmore() {
				// 滚动分页
			}
		},
		computed: {
			addClassName() {
				if (this.emptyStatus) {
					return 'u-flex u-flex-column u-flex-center u-flex-items-center'
				}
				return ''
			},
			emptyStatus() {
				if (this.orderList?.length !== 0) return false
				return true
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 100%;
		min-height: 100vh;
		background: #F6F6F6;

		/deep/ .u-tabs__wrapper {
			width: 750rpx;
			// height: 88rpx;
			height: 100%;
			background: #090D34;
			font-size: 28rpx;
			font-family: PingFangSC-Regular, PingFang SC;
			font-weight: 400;
			color: #EDDBC3;
			line-height: 40rpx;
		}
		/deep/ .uni-swiper-wrapper {
			height: 1800rpx !important;
		}

		.orderRecord {
			width: 100%;

			.orderinfo {
				width: 670rpx;
				height: 128rpx;
				background: #FFFFFF;
				border-radius: 16rpx;
				margin-top: 36rpx;
				padding: 0 24rpx;
				box-sizing: border-box;

				.name {
					width: 256rpx;
					font-family: PingFangSC-Medium, PingFang SC;

					.title {
						font-size: 30rpx;
						font-weight: 500;
						color: #060503;
						line-height: 42rpx;
					}

					.date {
						font-size: 24rpx;
						font-weight: 400;
						color: #9C9B98;
						line-height: 34rpx;
					}
				}

				.status {
					width: 144rpx;
					height: 40rpx;
					border-radius: 24rpx;
					font-size: 28rpx;
					font-weight: 500;
					margin-left: 56rpx;
				}

				.orangeBgColor {
					background: rgba(244, 138, 14, 0.15);
					color: #F48A0E;
					
				}
				

				.successBgColor {
					background: rgba(111, 208, 40, 0.15);
					color: #6FD028;
				}

				.errorBgColor {
					background: rgba(241, 107, 111, 0.15);
					color: #F16B6F
				}

				.money {
					margin-left: auto;
					font-size: 36rpx;
					font-family: PingFangSC-Semibold, PingFang SC;
					font-weight: 600;
					color: #B5804F;
					line-height: 50rpx;
					.smallText {
						font-size: 20rpx;
						font-family: PingFangSC-Semibold, PingFang SC;
						font-weight: 600;
						color: #B5804F;
						line-height: 28rpx;
					}
				}
			}
		}

		.order_list {
			/deep/ .u-cell__left-icon-wrap {
				width: 100rpx;
				margin-right: 12rpx;
			}
		}
	}
</style>
