<template>
	<view class="container">
		<view class="bgBox">
			<view class="userInfo">
				<view class="avatar">
					<!-- <image src="/static/icon/head.png" mode="aspectFill"></image> -->
					<image :src="'/static/icon/'+headStr+'.png'" mode="aspectFill"></image>
				</view>
				<view class="userName u-flex u-flex-column" v-if="isLogin">
					<text class="name">{{`HI，${getUserInfos.actual_name || ''}`}}</text>
					<text class="phone">{{getUserInfos.phone}}</text>
				</view>
				<view class="userName" v-else>
					<navigator open-type="navigate" url="/pages/login/login">
						登录
					</navigator>
				</view>
			</view>
		</view>


		<view class="memberLegal u-flex u-flex-center">
			<view class="memberEnjoy">
				<u-row justify="space-between">
					<u-col span="4" justify="center" v-for="(item,index) in memberNav" :key="index">
						<view class="demo-layout bg-purple iconList">
							<image :src="item.icon" mode="aspectFill" @click="clickNav(item)"></image>
							<view class="title">{{item.name}}</view>
						</view>
					</u-col>
				</u-row>
			</view>
		</view>

		<view class="listView">
			<u-cell-group :border='false'>
				<view v-for="(item,index) in cellList" :key="index">
					<u-cell :border='false' v-if="isLogin" :title="item.name" size="large" :name="item.enName" isLink
						:icon="item.icon" @click="handleListItem"
						titleStyle="font-size: 32rpx;font-weight: 400;color: #28334A;line-height: 44rpx;"
						rightIconStyle="font-size:14px;"></u-cell>
					<u-cell :border='false' v-else-if="!isLogin && !item.power" :title="item.name" size="large"
						:name="item.enName" isLink :icon="item.icon" @click="handleListItem"
						titleStyle="font-size: 32rpx;font-weight: 400;color: #28334A;line-height: 44rpx;"
						rightIconStyle="font-size:14px;"></u-cell>
				</view>

			</u-cell-group>
		</view>


		<!-- 弹窗 -->
		<view>
			<u-modal :show="showModal" :title="title" :confirmText="confirmText" @confirm="confirm"
				:showCancelButton="true" @cancel=" showModal = false" :content='content'></u-modal>
			<!-- <u-button @click="show = true">打开</u-button> -->
		</view>
		<Tarbar currentPage="mine"></Tarbar>
	</view>

</template>

<script>
	import {
		mapGetters,
		mapMutations
	} from 'vuex'
	import common from '@/utils/common'
	import store from "@/store"
	import Tabbar from '@/components/tabbar/Tarbar.vue'
	import {
		getQy,
		getUserInfo
	} from "@/config/api/user.js";
	import Tarbar from '@/components/tabbar/Tarbar.vue'

	export default {
		data() {
			return {
				memberNav: [{
					icon: '/static/icon/enjoy.png',
					path: '/pages/mine/privilege/privilege',
					name: '我的权益',
					page: 'current'
				}, {
					icon: '/static/icon/order.png',
					path: '/pages/mine/order/order',
					name: '我的订单',
					page: ''
				}, {
					icon: '/static/icon/customerService.png',
					path: '/pages/mine/service/service',
					name: '客服中心',
					page: ''
				}],
				cellList: [{
						icon: '/static/icon/icon_me_agreement.png',
						path: '',
						name: '查看协议',
						enName: 'look',
						power: false
					},
					{
						icon: '/static/icon/icon_me_about_us.png',
						path: '',
						name: '关于我们',
						enName: 'about',
						power: false
					},
					{
						icon: '/static/icon/icon_me_logoff.png',
						path: '',
						name: '注销账户',
						enName: 'logoff'
					},
					{
						icon: '/static/icon/icon_me_logout.png',
						path: '/pages/login/login',
						name: '退出登录',
						enName: 'logout',
						power: true
					}
				],
				showModal: false,
				title: '登录',
				content: '您好，请先完成登录！',
				confirmText: '去登录',
			};
		},
		components: {
			Tarbar,
		},
		methods: {
			...mapMutations(['LOGOUT']),
			orderPage(item) {
				uni.navigateTo({
					url: item.path
				})
			},

			clickNav(item) {
				if (!(store.state.user.token)) {
					this.showModal = true;
					return;
				}

				if (item.page) {
					getUserInfo({}).then(async (res) => {
						if (res.code === 100000) {
							if ((res?.data && res?.data.status === 4) || (res?.data && res?.data.status ===
									5)) {
								getQy({}).then((res) => {
									if (res.code === 100000) {
										uni.navigateTo({
											url: `/pages/webview/webview?urlPath=${encodeURIComponent(res?.data?.url)}`
										});
									} else {
										uni.$u.route(item.path);
									}
								}).catch((err) => {
									console.log(err, 'err');
								})
							} else {
								uni.$u.route(item.path);
							}

						}
					}).catch((err) => {
						console.log(err, 'err');
					})
					return
				}

				if (item.path) {
					uni.$u.route(item.path);
				}
			},
			handleListItem(item) {
				if (item.name === 'logout') {
					uni.$u.route({
						type: 'reLaunch',
						url: 'pages/login/login',
					})
					this.LOGOUT()
					return;
				}
				if (item.name === 'about') {
					uni.$u.route('/subpages/appPrivacyAgreement/appPrivacyAgreement')
					return;
				}
				if (item.name === 'look') {
					uni.$u.route('/subpages/assessAgreement/assessAgreement')
					return;
				}
			},
			confirm() {
				this.showModal = false;
				if (!(store.state.user.token)) {
					uni.$u.route('/pages/login/login');
					return;
				}
			},
		},
		onLoad() {
			uni.showLoading({

			})
		},
		onReady() {
			// #ifdef APP-PLUS
			plus.navigator.setStatusBarStyle('light');
			// #endif
		},
		onShow() {
			// #ifdef APP-PLUS
			plus.navigator.setStatusBarStyle('light');
			// #endif
			uni.hideLoading()
		},
		computed: {
			...mapGetters(['isLogin', 'getUserInfos']),
			headStr() {
				/**	getSexByBirthday
				 * @param idCard
				 * '0' 男
				 * '1' 女
				 */
				let sexNum = this.getUserInfos?.id_number ? common.getSexByBirthday(this.getUserInfos.id_number) : 2
				if (sexNum === '0') {
					return 'headman'
				}
				if (sexNum === '1') {
					return 'headwomen'
				}
				return 'head'
			},


		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 100%;
		height: 100vh;
		background: #F6F6F6;
	}

	.bgBox {
		width: 100%;
		// height: 400rpx;
		height: 384rpx;
		background: url(../../static/img/my_bg.png) no-repeat;
		background-size: cover;
		position: relative;

		.userInfo {
			position: absolute;
			top: 54rpx;
			left: 54rpx;
			display: flex;
			justify-content: center;
			align-items: center;

			.avatar {
				width: 132rpx;
				height: 132rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.userName {
				font-size: 28rpx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: #FFFFFF;
				line-height: 40rpx;
				margin: 0 26rpx;

				.name {
					font-size: 36rpx;
					font-family: Helvetica;
					color: #EDDBC3;
					line-height: 44rpx;
					margin-bottom: 4rpx;
				}

				.phone {
					font-size: 32rpx;
					font-family: Helvetica;
					color: #EDDBC3;
					line-height: 38rpx;
				}
			}
		}

	}

	.memberLegal {
		// padding: 0 32rpx;

		.memberEnjoy {
			width: 678rpx;
			height: 192rpx;
			margin-top: -114rpx;
			position: relative;
			z-index: 21;
			display: flex;
			background: #FFFFFF;
			box-shadow: 0px 4rpx 18rpx 0px rgba(0, 0, 0, 0.15);
			border-radius: 16rpx;

			.iconList {
				display: flex;
				flex-direction: column;
				align-items: center;
				justify-content: center;

				image {
					width: 80rpx;
					height: 80rpx;
				}

				.title {
					font-size: 24rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					line-height: 34rpx;
					color: #28334A;
				}
			}
		}


	}

	.listView {
		margin-top: 40rpx !important;
	}

	::v-deep .u-row {
		width: 100%;
	}

	/deep/ .u-cell-group {
		margin-top: 40rpx;
		margin-top: 12rpx !important;

		.u-cell__body {
			height: 120rpx;
			// border:1px solid hotpink;
		}
	}
</style>
