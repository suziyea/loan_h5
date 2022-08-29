<template>
	<view class="container">
		<view class="login_bg">
			<!-- <image src="/static/img/login_bg2.png" mode="aspectFill"></image> -->
			<view class="welcome_text">
				<view class="hello_text">
					Hello!
				</view>
				<view class="apptip_text">
					欢迎使用瑞信惠金融服务平台
				</view>
			</view>
		</view>
		
		<!-- 表单 -->
		<view class="form_view u-flex u-flex-center">
			<view class="form_list">
				<u--form
								labelPosition="left"
								:model="formContent"
								:rules="rules"
								ref="formContentRef"
						>
							<u-form-item
									label=""
									prop="formContent.phone"
									ref="item1"
							>
								<u--input
								prefixIcon="/static/icon/phone.png"
								placeholderClass="placeholderClass"
								v-model="formContent.phone"
								    placeholder="请输入手机号"
								    border="bottom"
								    clearable
									maxlength="13"
									@input="handleTelInput" 
									typx="tel"
								  ></u--input>
							</u-form-item>
							<u-form-item label="" prop="smsCode" ref="item2" >
								<!-- <u--input  v-model="phone" border ></u--input> -->
								<!-- 注意：由于兼容性差异，如果需要使用前后插槽，nvue下需使用u--input，非nvue下需使用u-input -->
								<!-- #ifndef APP-NVUE -->
								<u-input border="bottom" v-model="formContent.smsCode" type="number" maxlength='4' prefixIcon="/static/icon/smscode.png"
									placeholder="请输入验证码" placeholderClass="placeholderClass" prefixIconStyle="font-size: 44rpx;color: #909399">>
									<!-- #endif -->
									<!-- #ifdef APP-NVUE -->
									<u--input border="bottom" v-model="formContent.smsCode" type="number" maxlength='4' prefixIcon="/static/icon/smscode.png"
										placeholder="请输入验证码" placeholderClass="placeholderClass" prefixIconStyle="font-size: 44rpx;color: #909399">>
										<!-- #endif -->
										<template slot="suffix">
											<u-code ref="uCode" @change="codeChange" seconds="60" changeText="X秒重新获取"></u-code>
											<u-button @tap="getCode" :text="tips" :class="!success_verify ? 'disable_pointer' : '' "
												type="success" size="mini"></u-button>
										</template>
										<!-- #ifndef APP-NVUE -->
								</u-input>
								<!-- #endif -->
								<!-- #ifdef APP-NVUE -->
								</u--input>
								<!-- #endif -->
							</u-form-item>
						</u--form>
			</view>
		</view>
		
		<!-- 登录button -->
		<view class="btn_view u-flex u-flex-center">
			<view class="btn u-flex u-flex-center u-flex-items-center"  @click="clickSubmit">登录</view>
		</view>
		
		
		<!-- 登录提示协议 -->
		<view class="protocol_view">
			<view class="protocol">登录即表明您已经同意《平台用户协议》和《隐私协议》</view>
		</view>
		
		
	</view>
</template>

<script>
	import {
		login,
		sendSMS
	} from "@/config/api/user.js";
	import {
		mapMutations
	} from "vuex";
	export default {
		data() {
			return {
				seconds: 60,
				formContent: {
					smsCode: '',
					phone: ''
				},
				tips: '获取验证码',
				rules: {
					smsCode: [{
							required: true,
							message: '请输入手机验证码',
							trigger: ['blur']
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								return uni.$u.test.code(value, 4)
							},
							message: '手机验证码不正确',
							// 触发器可以同时用blur和change
							trigger: ['blur'],
						}
					],
					phone: [{
							required: true,
							message: '请输入手机号',
							trigger: ['blur'],
						},
						{
							// 自定义验证函数，见上说明
							validator: (rule, value, callback) => {
								// 上面有说，返回true表示校验通过，返回false表示不通过
								// uni.$u.test.mobile()就是返回true或者false的
								return uni.$u.test.mobile(value);
							},
							message: '手机号码不正确',
							// 触发器可以同时用blur和change
							trigger: ['blur'],
						}
					]
				}
			}
		},
		computed: {
			success_verify() {
				return uni.$u.test.mobile(this.formContent.phone);
			},
			sms_code_status() {
				return uni.$u.test.code(this.formContent.smsCode, 4);
			}
		},
		methods: {
			...mapMutations(['SETDEVICE', 'LOGIN', 'SET_TOKEN']),
			codeChange(text) {
				this.tips = text;
			},
			getCode() {
				console.log('你好啊---')
				if (this.$refs.uCode.canGetCode) {
					// 模拟向后端请求验证码
					uni.showLoading({
						title: '正在获取验证码'
					})
					// setTimeout(() => {
					// 	uni.hideLoading();
					// 	// 这里此提示会被this.start()方法中的提示覆盖
					// 	uni.$u.toast('验证码已发送');
					// 	// 通知验证码组件内部开始倒计时
					// 	this.$refs.uCode.start();
					// }, 2000);
					sendSMS({
							"phone": this.formContent.phone
						})
						.then((res) => {
							if (res.code === 100000) {
								uni.hideLoading();
								// 这里此提示会被this.start()方法中的提示覆盖
								uni.$u.toast('验证码已发送');
								// 通知验证码组件内部开始倒计时
								this.$refs.uCode.start();
								this.formContent.smsCodecode = "";
							}

						})
						.catch((err) => {
							uni.hideLoading();
							uni.showToast({
								icon: "none",
								title: err.msg || "获取验证码失败，请稍后再试",
							});
							this.iscode = true;
						});
				} else {
					uni.$u.toast('倒计时结束后再发送');
				}
			},
			change(e) {
				console.log('change', e);
			},
			clickSubmit() {
				uni.$u.debounce(this.submit, 500)
			},
			end() {
				uni.$u.toast('倒计时结束');
			},
			start() {
				uni.$u.toast('倒计时开始');
			},
			submit() {
				console.log('你好啊0大家都到家')
				this.$refs.formContentRef.validate().then(res => {
					let {
						phone,
						smsCode
					} = this.formContent
					// uni.$u.toast('校验通过')
					uni.showLoading({
						title: "加载中",
					});
					let loginHeaderObj = {
						osType: uni.$u.os(),
						deviceId: uni.$u.sys().deviceId,

					}
					this.SETDEVICE(loginHeaderObj)
					login({
							phone: phone,
							code: smsCode,
						}, {
							header: {
								'device-type': uni.$u.os() || '',
								'device-token': uni.$u.sys().deviceId || ''
							}
						})
						.then((res) => {
							uni.hideLoading();
							if (res.code === 100000) {
								let handleBaseInfo = {
									token: res.data.access_token,
									userInfo: res.data
								}
								this.LOGIN(handleBaseInfo)
								this.SET_TOKEN({
									token: res.data.access_token,
									refresh_token: res.data.refresh_token
								})
								uni.switchTab({
									url: '/pages/index/index'
								})
							}
						})
						.catch((err) => {
							uni.hideLoading();
							uni.showToast({
								icon: "none",
								title: err.msg || "登录失败，请稍后重试",
							});
						});

				}).catch(errors => {
					uni.$u.toast('校验失败')
				})
			},
			jumpContent(val) {
				if (val === 'platform') {
					uni.$u.route('/subpages/assessAgreement/assessAgreement')
					return;
				}

				if (val === 'hide') {
					uni.$u.route('/subpages/appPrivacyAgreement/appPrivacyAgreement')
					return;
				}
			},
			handleTelInput(e) {
				var len =  this.formContent.phone.length
				// var {phone} = this.formContent
				var reg = new RegExp("\\s", "g");
				var mobile_ = '';
				this.formContent.phone = this.formContent.phone.replace(reg, "");
				for (var i = 0; i < len; i++) {
					if (i == 2 || i == 6) {
						// charAt(int index)方法是一个能够用来检索特定索引下的字符的String实例的方法。
						//这里用来检索this.formContent.phone 的index为2和6
						mobile_ = mobile_ + this.formContent.phone.charAt(i) +
							" "; //当检索到2和6时，将原本的mobile_值加上新增的this.formContent.phone 值再加一个" "后再赋值给mobile_自己
					} else {
						mobile_ += this.formContent.phone.charAt(i);
					}
				}
				this.formContent.phone = mobile_
			},
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		width: 750rpx;
		// height: 1496rpx;
		height: 100vh;
		background: #090D34;
		.login_bg {
			position: relative;
			width: 750rpx;
			height: 480rpx;
			background: url(/static/img/login_bg2.png) no-repeat;
			background-size: cover;
			
			.welcome_text {
				position: absolute;
				left: 60rpx;
				top: 60rpx;
				.hello_text {
					width: 132rpx;
					height: 66rpx;
					font-size: 48rpx;
					font-family: PingFangSC-Semibold, PingFang SC;
					font-weight: 600;
					color: #EDDBC3;
					line-height: 66rpx;
					margin-bottom: 16rpx;
				}
				.apptip_text {
					width: 336rpx;
					height: 40rpx;
					font-size: 28rpx;
					font-family: PingFangSC-Regular, PingFang SC;
					font-weight: 400;
					color: #EDDBC3;
					line-height: 40rpx;
				}
			}
			
		}
		
		.form_view {
			.form_list {
				width: 662rpx;
				height: 320rpx;
				z-index: 12;
				background: linear-gradient(360deg, #F9F9F7 0%, #FDF1E5 100%);
				border-radius: 16rpx;
				margin-top: -190rpx;
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
					
				}
				
				
			}
		}
		
		.btn_view {
			margin-top: 140rpx;
			.btn {
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
		
		.protocol_view {
			margin: 68rpx 74rpx;
			.protocol {
				font-size: 24rpx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: #EDDBC3;
				line-height: 34rpx;
			}
			
		}
	}
</style>
