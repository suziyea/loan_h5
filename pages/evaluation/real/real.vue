<template>
	<view class="container u-flex u-flex-column u-flex-items-center ">
		<!-- <view class="bgBox">
			<view class="realImg">
				<image src="/static/img/real.png" mode="aspectFill"></image>
			</view>
			<view class="titleBox">
				<text class="title">实名认证</text>
				<text class="desc">为配合国家监管要求，请完成实名认证。</text>
			</view>
		</view> -->

		<!-- 		<view class="formList">
			<u--form :model="formContent" :rules="rules" ref="uForm" :errorType="errorType">
				<u-form-item label="" prop="name">
					<text class="form__title">真实姓名</text>

					<u-input v-model="formContent.name" placeholder="请如实填写您的姓名" border="bottom"
						placeholderStyle='font-size: 28rpx;font-family: PingFangSC-Regular, PingFang SC;font-weight: 400;color: #CBCBCB;line-height: 40rpx;'
						clearable />
				</u-form-item>
				<u-form-item label="" prop="idcard">
					<text class="form__title">身份证号</text>
					<u--input v-model="formContent.idcard" type="idcard" placeholder="请输入身份证号" border="bottom"
						placeholderStyle='font-size: 28rpx;font-family: PingFangSC-Regular, PingFang SC;font-weight: 400;color: #CBCBCB;line-height: 40rpx;'
						clearable>
					</u--input>
				</u-form-item>
			</u--form>
		</view>
		<view class="btn">
			<u-button type="primary" :plain="true" class="custom-style" @tap="clickSubmit" :hairline="true" text="下一步">
			</u-button>
		</view> -->

		<view class="img_bg">
			<view class="copywriting">
				<view class="bigtitle">
					实名认证
				</view>
				<view class="desc">
					为配合国家监管要求，请完成实名认证。
				</view>
			</view>
		</view>

		<view class="submit_view u-flex u-flex-column ">
			<view class="top_view u-flex  u-flex-items-center ">
				使用有效身份证件信息认证
			</view>
			<view class="form_info u-flex   u-flex-column  u-flex-items-center  ">
				<view class="form_list">
					<u--form  labelWidth='100' borderBottom :model="formContent" :rules="rules" ref="formContentRef">
						<!-- <u-form-item label="姓名" prop="actual_name" borderBottom ref="item1" style="margin-top: 12rpx;">
							<u--input inputAlign="right" v-model="formContent.actual_name" border="none" placeholder=""
								placeholderClass="placeholderClass" suffixIcon="/static/icon/my_name.png"></u--input>
						</u-form-item> -->
						<u-form-item  label="真实姓名" borderBottom prop="formContent.name" ref="item1">
							<u--input inputAlign="right"  borderBottom placeholderClass="placeholderClass"
								v-model="formContent.name" placeholder="请如实填写您的姓名" border="none" clearable></u--input>
						</u-form-item>
						<u-form-item label="身份证号" borderBottom prop="formContent.idcard" ref="item1">
							<u--input inputAlign="right" borderBottom placeholderClass="placeholderClass"
								v-model="formContent.idcard" placeholder="请输入身份证号" border="none" clearable></u--input>
						</u-form-item>
					</u--form>
				</view>
				<view class="btns u-flex u-flex-center  u-flex-items-center ">
					下一步
				</view>
			</view>

			<!-- <view class="btn">
				下一步
			</view> -->

		</view>

	</view>
</template>

<script>
	import {
		HandleRealName
	} from "@/config/api/user.js";
	export default {
		data() {
			return {
				errorType: 'toast',
				formContent: {
					phone: ''
				},
				rules: {
					name: [{
							required: true,
							message: '请输入姓名',
							// trigger: ['blur', 'change']
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// uni.$u.test.mobile()就是返回true或者false的
								return uni.$u.test.chinese(value);
							},
							message: '只能输入汉字',
							// 触发器可以同时用blur和change
							// trigger: ['change', 'blur'],
						}
					],
					idcard: [{
							required: true,
							message: '请输入身份证号',
							trigger: ['change', 'blur'],
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// uni.$u.test.mobile()就是返回true或者false的
								return uni.$u.test.idCard(value);
							},
							message: '身份证号码不正确',
							// 触发器可以同时用blur和change
							trigger: ['change', 'blur'],
						}
					]
				}
			}
		},
		computed: {
			realname_verify() {
				return uni.$u.test.chinese(this.formContent.name);
			},
			idcard_verify() {
				return uni.$u.test.idCard(this.formContent.idcard);
			}
		},
		methods: {
			clickSubmit() {
				uni.$u.debounce(this.submit, 500)
			},
			submit() {
				this.$refs.uForm.validate().then(res => {
					let {
						name,
						idcard
					} = this.formContent

					HandleRealName({
						"actual_name": name,
						"id_number": idcard
					}).then(async (res) => {
						if (res.code === 100000) {
							uni.removeStorageSync('userInfo');
							await this.$store.dispatch('setCurrentUserInfo')
							uni.$u.route('/pages/evaluation/addBank/addBank');
						}

					}).catch((err) => {
						console.log(err, 'err');
					})

				}).catch(errors => {
					uni.$u.toast(errors[0].message)
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		height: 100vh;
		width: 750rpx;
		background: #090C34;

		.img_bg {
			position: relative;
			width: 750rpx;
			height: 480rpx;
			background: url(../../../static/img/real_name_bg.png) no-repeat;
			background-size: cover;
			border: 1px solid skyblue;
			box-sizing: border-box;

			.copywriting {
				position: absolute;
				// margin: 36rpx 60rpx;

				top: 36rpx;
				left: 60rpx;
				color: #EDDBC3;
				font-family: PingFangSC-Semibold, PingFang SC;

				.bigtitle {
					font-size: 48rpx;
					font-weight: 600;
					line-height: 66rpx;
					margin-bottom: 20rpx;
				}

				.desc {
					font-size: 24rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #EDDBC3;
					line-height: 34rpx;
				}
			}
		}

		.submit_view {
			width: 662rpx;
			margin-top: -340rpx;

			.top_view {
				box-sizing: border-box;
				position: relative;
				z-index: 3;
				width: 100%;
				height: 100rpx;
				padding: 0 32rpx;
				// margin-bottom: @at-root ;
				top: 64rpx;
				background: #EDDBC3;
				border-radius: 16rpx 16rpx 0rpx 0rpx;
				font-size: 30rpx;
				font-family: PingFangSC-Medium, PingFang SC;
				font-weight: 500;
				color: #873D0A;
				line-height: 42rpx;
			}

			.form_info {
				z-index: 2;
				width: 662rpx;
				height: 480rpx;
				background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				border-radius: 16rpx;
				box-sizing: border-box;

				.form_list {
					width: 662rpx;
					height: 320rpx;
					border-radius: 16rpx;
					margin-top: 72rpx;

					/deep/ .u-form {
						margin: 24rpx 32rpx;

						.u-button--success {
							border: none;
							font-size: 30rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #8C5E2D !important;
							line-height: 42rpx;
							background: none;
						}
						.u-form-item__body {
							padding: 36rpx 0 !important;
						}
						.u-form-item__body__left__content__label {
							width: 224rpx;
							font-size: 28rpx;
							font-family: PingFangSC-Regular, PingFang SC;
							font-weight: 400;
							color: #5A5757 !important;
							line-height: 40rpx;
						}
					}
				}
				.btns {
					width: 570rpx;
					height: 88rpx;
					background: linear-gradient(180deg, #EDDFC5 0%, #CCAC83 100%);
					border-radius: 44rpx;
					margin-top: -48rpx;
				}
			}
		}

		.bgBox {
			width: 100%;
			height: 440rpx;
			background: url(../../../static/img/real_bg.png) no-repeat;
			background-size: cover;
			position: relative;
			margin-bottom: 88rpx;

			.realImg {
				width: 200rpx;
				height: 200rpx;
				position: absolute;
				top: 166rpx;
				right: 32rpx;

				image {
					width: 100%;
					height: 100%;
				}
			}

			.titleBox {
				width: 460rpx;
				height: 200rpx;
				position: absolute;
				top: 208rpx;
				left: 56rpx;
				display: flex;
				flex-direction: column;

				.title {
					font-size: 42rpx;
					font-family: PingFangSC-Medium, PingFang SC;
					font-weight: 500;
					color: #FFFFFF;
					line-height: 58rpx;
					margin-bottom: 8rpx;
				}

				.desc {
					font-size: 24rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #FFFFFF;
					line-height: 34rpx;
				}
			}

		}

		.formList {
			padding: 0 56rpx;

			.formItem {
				margin-bottom: 52rpx;

				.realName {
					font-size: 36rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #414141;
					line-height: 50rpx;
				}
			}

			.form__title {
				height: 50rpx;
				font-size: 36rpx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: #414141;
				margin-bottom: 24rpx;
				line-height: 50rpx;
			}
		}

		.btn {
			padding: 0 52rpx;
			margin-top: 160rpx;

			.custom-style {
				background: #4579E6;
				border-radius: 8rpx;
				font-size: 32rpx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: #FFFFFF;
				line-height: 44rpx;
			}
		}

		/deep/ .u-form-item__body__right__content__slot {
			flex-direction: column;
			align-items: flex-start;
		}

		/deep/ .u-input {
			width: 100%;
			box-sizing: border-box;
		}
	}

	.disableColor {
		background: #f5f5f5 !important;
		pointer-events: none;
		border: 1px solid #ccc;
		color: #000 !important;
	}
</style>
