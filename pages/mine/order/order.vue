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
		<u-sticky bgColor="#fff">
			<u-tabs lineColor="#EDDBC3" lineWidth="80rpx" :scrollable="false" :list="list1"
				itemStyle="width:250rpx; height: 88rpx; padding:0rpx;"
				activeStyle="font-size: 28rpx;font-weight: 500;color: #EDDBC3;" @click="click">

			</u-tabs>
		</u-sticky>
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
				list1: [{
					name: '全部',
				}, {
					name: '已支付',
				}, {
					name: '未支付'
				}],
				ak: ''
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
				this.ak = item;
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

		/deep/ .u-tabs__wrapper {
			width: 750rpx;
			height: 88rpx;
			background: #090D34;
			font-size: 28rpx;
			font-family: PingFangSC-Regular, PingFang SC;
			font-weight: 400;
			color: #EDDBC3;
			line-height: 40rpx;
		}

		.order_list {
			/deep/ .u-cell__left-icon-wrap {
				width: 100rpx;
				margin-right: 12rpx;
			}
		}
	}
</style>
