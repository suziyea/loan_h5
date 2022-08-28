<template>
	<view class="container_home u-flex u-flex-column ">
		<view class="top">
			<view class="home_tips u-flex u-flex-column u-flex-items-center">
				<view class="tip_text">
					最高借贷额度(元)
				</view>
				<view class="amount">
					<u-count-to :endVal="loan_amount" separator="," class="count"></u-count-to>
				</view>
			</view>

			<!-- 首页背景图 -->
			<view class="home_bg  u-flex u-flex-center">
				<view class="product_feature u-flex u-flex-column u-flex-center u-flex-items-center">
					<u-row justify="space-between">
						<u-col span="4" justify="center" v-for="(item,index) in memberNav" :key="index">
							<view class="demo-layout bg-purple iconList" @click="setModalText">
								<image :src="item.icon" mode="aspectFill"></image>
								<view class="title">{{item.name}}</view>
							</view>
						</u-col>
					</u-row>
					<view class="borrow_btn u-flex u-flex-center u-flex-items-center">
						我要借钱
					</view>
				</view>


			</view>

			<view class="product_advantage u-flex u-flex-center u-flex-items-center">
				<u-row justify="space-between">
					<u-col span="4" justify="center" v-for="(item,index) in productDelsNav" :key="index">
						<view class="demo-layout bg-purple icontip">
							<view class="productBox">
								<image :src="item.icon" @click="setModalText"></image>
								<view class="tiptext">{{item.tips}}</view>
							</view>
							<view class="title">{{item.name}}</view>
						</view>
					</u-col>
				</u-row>
			</view>

			<!-- 我要 -->
			<view class="takeatest u-flex u-flex-center">
				<view class="takeatest_bg  u-flex u-flex-center u-flex-items-center">
					<view class="left">
						<view class="title">
							测测您 能贷多少钱
						</view>
						<view class="desc">
							根据资质信息,为您提供相应的贷款
						</view>
						<view class="btn u-flex u-flex-center u-flex-items-center">
							我要测评
						</view>
					</view>

					<view class="right">
						<image src="/static/img/home_take_go.png" mode="aspectFill"></image>
					</view>

				</view>

			</view>
		</view>
		<view class="bottom u-flex u-flex-center u-flex-items-center">
			<image src="/static/icon/home_deng.png" mode="aspectFill"></image>
			评估如实反映您的信用情况，最终结果以第三方审核为准
		</view>

		<!-- 弹窗 -->
		<view>
			<u-modal :show="showModal" :title="title" :confirmText="confirmText" @confirm="confirm"
				:showCancelButton="true" @cancel=" showModal = false" :content='content'></u-modal>
		</view>

	</view>
</template>

<script>
	import store from "@/store"
	import {
		getEdu
	} from "@/config/api/user.js";
	import {
		mapGetters,
	} from 'vuex'
	export default {
		props: {
			// 检测类型 + 其他验证
			userStatus: {
				type: Number,
				default: 0,
				required: true,
				validator: function(value) {
					return value >= 0
				}
			}
		},
		data() {
			return {
				title: 'Hello',
				messageArr: ['评估如实反映您的信用情况，最终结果以第三方审核为准', '评估如实反映您的信用情况，最终结果以第三方审核为准', '评估如实反映您的信用情况，最终结果以第三方审核为准'],
				showModal: false,
				title: '实名认证',
				content: '您好，请先完成实名认证信息补全!',
				confirmText: '去绑卡',
				loan_amount: '',
				memberNav: [{
					icon: '/static/icon/home_money.png',
					path: '',
					name: '随时可还'
				}, {
					icon: '/static/icon/home_apply.png',
					path: '',
					name: '极简申请'
				}, {
					icon: '/static/icon/home_organ.png',
					path: '',
					name: '合法机构'
				}],
				productDelsNav: [{
					icon: '/static/icon/home_deng.png',
					tips: '利息最低',
					name: '0.02%'
				}, {
					icon: '/static/icon/home_deng.png',
					tips: '分期灵活',
					name: '12-36期'
				}, {
					icon: '/static/icon/home_deng.png',
					tips: '极速放款',
					name: '10分钟'
				}],
			}
		},
		onLoad(option) {
			console.log(option);

		},
		created() {
			this.getEdus()
			this.setModalText()
		},
		methods: {
			getEdus() {
				getEdu({
					"code": "loan_amount"
				}).then((res) => {
					if (res.code === 100000) {
						this.loan_amount = res?.data?.value?.value || '****'
					}
				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			borrowMoney() {
				this.setModalText()
			},
			setModalText() {
				const storeToken = uni.getStorageSync('token');
				const storeUserInfo = uni.getStorageSync('userInfo');
				if (!(storeToken) && !storeUserInfo) {
					this.showModal = true;
					this.title = '登录';
					this.content = '您好，请先完成登录！';
					this.confirmText = '去登录'
				}

				if (this.userStatus === 1) {
					this.showModal = true;
					this.title = '实名认证';
					this.content = '您好，请先完成实名认证信息补全!';
					this.confirmText = '去实名'
				}

				if (this.userStatus === 2) {
					this.showModal = true;
					this.title = '绑定银行卡';
					this.content = '您好，为了方便的贷款，请绑定银行卡！';
					this.confirmText = '去绑卡'
				}

				if (this.userStatus === 3) {
					this.showModal = true;
					this.title = '评估';
					this.content = '您好，为了方便的贷款，请完成评估！';
					this.confirmText = '去评估'
				}

				if (this.userStatus === 4) {
					this.showModal = true;
					this.title = '评估结果';
					this.content = '您好，为了方便的贷款，请完成二次评估！';
					this.confirmText = '去评估'
				}
			},
			confirm() {
				this.showModal = false;
				if (!store.state.user.token) {
					uni.$u.route('/pages/login/login');
					return;
				}
				if (this.userStatus === 1) {
					uni.$u.route('/pages/evaluation/real/real');
				}
				if (this.userStatus === 2) {
					uni.$u.route('/pages/evaluation/addBank/addBank');
				}
				if (this.userStatus === 3) {
					uni.$u.route('/pages/product/evaluationFirtPay/evaluationFirtPay');
				}
				if (this.userStatus === 4) {
					uni.$u.route('/pages/product/reflect/reflect');
				}
			},
			clickEvaluation() {
				this.setModalText()
			}
		},
	}
</script>

<style lang="scss" scoped>
	.container_home {
		width: 750rpx;
		height: 1496rpx;
		background: #090C34;
		box-sizing: border-box;

		.top {
			.home_tips {
				height: 218rpx;
				box-sizing: border-box;
				margin: 0 40rpx;

				.tip_text {
					padding: 26rpx 0 20rpx 0;
					font-size: 28rpx;
					font-family: PingFangSC-Medium, PingFang SC;
					font-weight: 500;
					color: #F7DA87;
					line-height: 40rpx;
					background: linear-gradient(180deg, #E7D1AB 0%, #E3AA8E 100%);
					-webkit-background-clip: text;
					-webkit-text-fill-color: transparent;
				}

				.amount {
					.count {
						font-size: 120rpx !important;
						font-family: DINAlternate-Bold, DINAlternate;
						font-weight: bold;
						color: #FFFFFF;
						line-height: 70px;
						background: linear-gradient(180deg, #E7D1AB 0%, #E3AA8E 100%);
						-webkit-background-clip: text;
						-webkit-text-fill-color: transparent;
					}
				}
			}

			.home_bg {
				position: relative;
				width: 750rpx;
				height: 660rpx;
				background: url(/static/img/home_bg.png) no-repeat;
				background-size: cover;

				.product_feature {
					margin-top: 300rpx;
					width: 662rpx;
					height: 360rpx;
					background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
					border-radius: 16rpx;

					.iconList {
						display: flex;
						flex-direction: column;
						align-items: center;
						justify-content: center;

						.title {
							font-size: 28rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #282626;
							line-height: 40rpx;
						}

						image {
							width: 72rpx;
							height: 72rpx;
						}
					}

					.borrow_btn {
						width: 570rpx;
						height: 88rpx;
						margin-top: 44rpx;
						background: linear-gradient(180deg, #FF5F00 0%, #F48B45 100%);
						border-radius: 44rpx;
						font-size: 32rpx;
						font-family: PingFangSC-Medium, PingFang SC;
						font-weight: 500;
						color: #FFFFFF;
						line-height: 44rpx;
					}
				}




			}

			.product_advantage {
				width: 100%;
				height: 176rpx;

				.icontip {
					display: flex;
					flex-direction: column;
					align-items: center;
					justify-content: center;

					.productBox {
						display: flex;
						align-items: center;
						justify-content: center;

						image {
							width: 40rpx;
							height: 40rpx;
						}

						.tiptext {
							font-size: 30rpx;
							font-family: PingFangSC-Medium, PingFang SC;
							font-weight: 500;
							color: #EDDBC3;
							line-height: 42rpx;
							// -webkit-background-clip: text;
							// -webkit-text-fill-color: transparent;
							margin-left: 8rpx;
						}
					}

					.title {
						font-size: 24rpx;
						font-family: PingFangSC-Medium, PingFang SC;
						font-weight: 500;
						color: #EDDBC3;
						line-height: 34rpx;
						// -webkit-background-clip: text;
						// -webkit-text-fill-color: transparent;
					}
				}
			}

			.takeatest {
				width: 100%;
				height: 178rpx;

				.takeatest_bg {
					position: relative;
					width: 678rpx;
					height: 178rpx;
					background: url(../../../static/img/home_takeatest_bg.png) no-repeat;
					background-size: cover;

					.left {
						padding-left: 72rpx;
						color: #EDDBC3;
						font-family: PingFangSC-Medium, PingFang SC;
						font-weight: 500;

						.title {
							font-size: 28rpx;
							line-height: 40rpx;
						}

						.desc {
							font-size: 24rpx;
							line-height: 34rpx;
						}

						.btn {
							width: 140rpx;
							height: 40rpx;
							margin-top: 14rpx;
							background: linear-gradient(180deg, #E7D1AB 0%, #E3AA8E 100%);
							border-radius: 44rpx;
							font-size: 24rpx;
							color: #873D0A;
							line-height: 34rpx;
						}
					}

					.right {
						margin-left: auto;

						image {
							width: 176rpx;
							height: 108rpx;
						}
					}

				}
			}
		}

		.bottom {
			display: flex;
			margin-top: auto;
			width: 100%;
			height: 60rpx;
			background: #222455;
			font-size: 24rpx;
			font-family: PingFangSC-Medium, PingFang SC;
			font-weight: 500;
			color: #555A7A;
			line-height: 34rpx;

			image {
				width: 24rpx;
				height: 28rpx;
				margin-right: 4rpx;
			}
		}
	}

	::v-deep .u-row {
		width: 100%;
	}
</style>
