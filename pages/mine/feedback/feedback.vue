<template>
	<view class="feedback">
		<!-- <view class="group">
			<view class="item">
				<view class="title">联系人</view>
				<view class="input"><input type="text" v-model="contact" placeholder="请输入您的姓名"
						placeholder-class="myholder" /></view>
			</view>
			<view class="item">
				<view class="title">联系电话</view>
				<view class="input"><input type="number" v-model="contact_phone" placeholder="请输入您的手机号码"
						placeholder-class="myholder" /></view>
			</view>
			<view class="item">
				<view class="title">联系邮箱</view>
				<view class="input"><input type="text" inputAlign="right" v-model="contact_email" placeholder="请输入您的邮箱"
						placeholder-class="myholder" /></view>
			</view>
		</view>
		<view class="block"></view>
		<view class="group">
			<view class="textarea">
				<view class="title">反馈内容</view>
				<view class="content">
					<textarea v-model="content" maxlength="-1" placeholder="如果您对我们的商品、服务、APP有什么意见或建议， 请在这里告诉我们"
						placeholder-class="myholder" />
				</view>
			</view>
		</view>
		<view class="btn">
			<u-button type="primary" :plain="true" class="custom-style" @tap="clickSubmit" :hairline="true" text="下一步">
			</u-button>
		</view> -->
		<view class="submitInformation u-flex u-flex-center">
			<view class="formInfo">
				<u--form :model="formContent" ref="uForm" labelWidth="auto">
					<u-form-item label="联系人" prop="actual_name" borderBottom ref="item1">
						<u--input inputAlign="right" v-model="formContent.actual_name" border="none" placeholder="请输入姓名"
							placeholderClass="placeholderClass"></u--input>
					</u-form-item>
					<u-form-item label="联系电话" prop="id_number" borderBottom ref="item2">
						<u--input inputAlign="right" v-model="formContent.id_number" border="none" placeholder="请输入身份证号"
							placeholderClass="placeholderClass"></u--input>
					</u-form-item>
					<u-form-item label="联系邮箱" prop="id_number" ref="item2">
						<u--input inputAlign="right" v-model="formContent.id_number" border="none" placeholder="请输入身份证号"
							placeholderClass="placeholderClass"></u--input>
					</u-form-item>
					<view class="fill"></view>
				<u-form-item label="反馈内容" prop="id_number" ref="item2">
					</u-form-item>
						<u--textarea  border="none" :placeholderClass="textareaClass"  v-model="formContent.code" placeholder="如果您对我们的产品,服务,APP有什么意见或建议,请在这里告诉我们" ></u--textarea> 

				</u--form>

				<view class="btn_view u-flex u-flex-center ">
					<view class="btnContent u-flex u-flex-center  u-flex-items-center ">
						下一步
					</view>
				</view>
			</view>
		</view>
		<u-toast ref="uToast"></u-toast>
	</view>
</template>

<script>
	import {
		addFeedback
	} from "@/config/api/user.js";
	export default {
		data() {
			return {
				contact: '',
				contact_phone: '',
				contact_email: '',
				content: '',
				formContent: {
					actual_name: '',
					id_number: '',
					bank_name: '',
					bank_id: '',
					reserve_phone: '',
					code: '',
				},
			};
		},
		methods: {
			clickSubmit() {
				uni.$u.debounce(this.submit, 500)
			},

			submit() {
				if (!this.contact) {
					this.showNoneIconToast('请输入您的姓名！')
					return
				}
				if (!this.contact_phone) {
					this.showNoneIconToast('请输入您的手机号码！')
					return
				}
				if (!this.content) {
					this.showNoneIconToast('请填写反馈内容！')
					return
				}
				if (!uni.$u.test.mobile(this.contact_phone)) {
					this.showNoneIconToast('请输入正确的手机号！')
					return
				}
				if (this.contact_email) {
					if (!uni.$u.test.email(this.contact_email)) {
						this.showNoneIconToast('请输入正确的邮箱格式！')
						return
					}
				}

				addFeedback({
					"contact": this.contact,
					"contact_phone": this.contact_phone,
					"contact_email": this.contact_email,
					"content": this.content
				}).then((res) => {
					if (res.code === 100000) {
						let params = {
							message: "我们已收到反馈内容，会尽快处理",
							url: '/pages/mine/mine'
						}
						this.$refs.uToast.show({
							...params,
							complete() {
								params.url && uni.switchTab({
									url: params.url
								})
							}
						})
						// this.showNoneIconToast('我们已收到反馈内容，会尽快处理')
						// uni.switchTab({
						// 	url: '/pages/mine/mine'
						// })
					}

				}).catch((err) => {
					console.log(err, 'err');
				})
			},
			showNoneIconToast(title) {
				uni.showToast({
					icon: 'none',
					title: title
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	page {
		width: 100%;
		width: 750rpx;
		height: 100vh;

		background: #F6F6F6;

		.submitInformation {
			width: 100%;
			z-index: 2;
				padding: 0rpx 44rpx;
				box-sizing: border-box;
			.formInfo {
				// width: 662rpx;
				// height: 916rpx;
				// background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				// border-radius: 16rpx;

				.fill {
					width: 750rpx;
					height: 12rpx;
					background: #EDDBC3;
					opacity: 0.29;
					margin: 0rpx -32rpx;
				}

				.btn_view {
					width: 100%;
					margin-top: 134rpx;
					box-sizing: border-box;

					.btnContent {
						width: 662rpx;
						height: 88rpx;
						background: linear-gradient(180deg, #EDDFC5 0%, #CCAC83 100%);
						border-radius: 44rpx;
					}
				}
				/deep/ .u-textarea  {
					width: 662rpx;
					height: 306rpx !important;
					border-radius: 16rpx;
					border: 2rpx solid #E8E8E8 !important;
				}
				
				.u-textarea__field {
height: 100%;
				}
				

				/deep/ .u-form {
					// margin: 24rpx 32rpx;
					.placeholderClass {
						color: #9C9B98 !important;
					}
					 .textareaClass {
					color: red !important;
				}



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
						padding: 30rpx 0 !important;
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
		}

		.feedback {
			width: 100%;
			height: 100%;
			position: fixed;
			background-color: #f7f7f7;

			.block {
				width: 100%;
				height: 20rpx;
			}

			.group {
				background-color: #FFFFFF;

				.myholder {
					color: #cccccc;
					font-size: $mc-normal-font-size;
				}

				.item {
					@extend %flex-layout-row;
					height: 100rpx;
					line-height: 100rpx;
					justify-content: space-between;
					margin-left: 45rpx;
					border-bottom: 1rpx solid #e6e6e6;

					.title {
						flex: 0 0 121rpx;
						font-size: $mc-normal-font-size;
						color: $mc-normal-font-color;
					}

					.input {
						flex: 1;
						text-align: left;
						margin-left: 72rpx;

						input {
							padding-right: 20rpx;
						}
					}
				}

				:last-of-type {
					border-bottom: none;
				}

				.textarea {
					.title {
						font-size: $mc-normal-font-size;
						color: $mc-normal-font-color;
						margin-left: 45rpx;
						padding: 33rpx 0 28rpx 0;
					}

					.content {
						padding: 0 46rpx 46rpx 46rpx;

						textarea {
							width: 100%;
						}
					}

				}
			}

			.submit {
				width: 640rpx;
				height: 88rpx;
				background: linear-gradient(-90deg, #FF7611, #FF9001);
				border-radius: 44rpx;
				position: fixed;
				bottom: 19rpx;
				z-index: 1;
				left: 55rpx;
				right: 55rpx;
				color: #FFFFFF;
				font-size: $mc-large-font-size;
			}
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
</style>
