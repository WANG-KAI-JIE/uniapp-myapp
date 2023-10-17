<template>
	<view class="container container25504">
		<view class="flex flex-wrap diygw-col-24 flex-direction-column"> </view>
		<view class="flex diygw-col-24 flex-direction-column">
			<view class="diygw-tabs text-center solid-bottom justify-center tabs-title">
				<view class="diygw-tab-item tabs-item-title flex-sub" :class="index == tabsIndex ? ' cur text-green ' : ''" v-for="(item, index) in tabsDatas" :key="index" @click="changeTabs" :data-index="index"> <text v-if="item.icon" :class="item.icon"></text> {{ item.text }} </view>
			</view>
			<view class="">
				<view v-if="tabsIndex == 0" class="flex-sub">
					<view class="flex diygw-col-24 noticebar-clz">
						<diy-noticebar class="flex1 diy-notice-bar" color="#db6055" bgColor="#fff" leftIcon="diy-icon-notification">
							<block v-slot:content>
								<text v-if="showFlag" class="diy-notice-item"> {{notice}} </text>
								<text v-else class="diy-notice-item">系统正在维护中</text>
							</block>
						</diy-noticebar>
					</view>
					<view class="diygw-col-24">
						<view class="diygw-search">
							<view class="flex1 align-center flex padding-xs solid radius">
								<text style="color: #555 !important" class="diy-icon-search"></text>
								<input class="flex1" name="search" type="" v-model="search" placeholder="请输入搜索内容" />
							</view>
						</view>
					</view>

					<view v-for="(share,index) in shares" :key="index">
						<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex1-clz">
							<view class="flex diygw-col-24 justify-around avatar-clz">
								<view class="diygw-avatar radius-md bg-none">
									<image mode="aspectFill" class="diygw-avatar-img radius-md" :src="share.cover"></image>
								<text 
									style="position: absolute;left: -15px;top: -5px;background-color: red;font-size: 13px;padding: 0 5px;"
									v-if="share.isOriginal">原创</text>
								<text v-else
									style="position: absolute;left: -15px;top: -5px; background-color: green;font-size: 13px;padding: 0 5px;">
									转载</text>
								</view>
							</view>
							<view class="flex flex-wrap diygw-col-24 flex-clz">
								<view class="diygw-col-24 text-clz"> {{share.title}} </view>
								<view class="diygw-text-line2 diygw-col-24 text1-clz"> {{share.summary}} </view>
							</view>
							<view class="flex flex-wrap diygw-col-24 flex-direction-column justify-center flex3-clz">
								<view class="diygw-col-24 text3-clz"> {{share.price}} 积分 </view>
								<view class="diygw-col-24" v-if="share.downloadUrl" @tap="gotoDetail(share.id)"> 下载 </view>
								<view class="diygw-col-24" v-else @tap="gotoExchange(share.id)"> 兑换 </view>
							</view>
						</view>
						<view class="flex diygw-col-24">
							<view class="diygw-pzx" style="border-bottom: 1px solid #eee"></view>
						</view>
				</view>
			</view>	
				<view v-if="tabsIndex == 1" class="flex-sub">
					<view class="flex flex-wrap diygw-col-24 flex-direction-column flex8-clz">
						<view class="flex flex-wrap diygw-col-24 justify-center flex6-clz">
							<view class="diygw-col-24 red text8-clz diygw-text-xl"> 01 </view>
							<view class="flex flex-wrap diygw-col-24 flex-direction-column flex7-clz">
								<view class="diygw-text-line1 diygw-col-24 text9-clz"> 积分获得方式 </view>
								<view class="flex diygw-col-24 line2-clz">
									<view class="diygw-pzx" style="border-bottom: 2px solid #f3b23f"></view>
								</view>
							</view>
						</view>
						<u-form-item labelWidth="auto" class="diygw-col-24" labelPosition="top" prop="textarea">
							<u-input maxlength="200" height="60px" class="" placeholder="" v-model="textarea" type="textarea"></u-input>
						</u-form-item>
					</view>
				</view>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
		import {
			request,	
		} from '@/utils/request';
		
		import{
			LATEST_NOTICE_URL,
			SHARE_LIST_URL
		} from '@/utils/api';
		
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				notice: "系统正在维护中...",
				showFlag: null,
				tabsDatas: [
					{ text: `发现`, icon: `diy-icon-discover` },
					{ text: `使用说明`, icon: `diy-icon-text` }
				],
				tabsLeft: 0,
				tabsWidth: 0,
				tabsItemWidth: 0,
				tabsIndex: 0,
				search: '',
				textarea: '每日签到投稿',
				shares: [],				
				pageNo: 1,
				pageSize: 8,
				more: true
			};
		},
		onShow() {
			this.setCurrentPage(this);
			this.getNotice();
			this.getShares(true);
		},
		onLoad(option) {
			this.setCurrentPage(this);
			if (option) {
				this.setData({
					globalOption: this.getOption(option)
				});
			}
			this.init();
		},
		// 下拉刷新
		onPullDownRefresh() {
			// 参数为 true，就重新开始加载
			this.getShares(true);
		},
		// 触底加载更多
		onReachBottom() {
			// 没有更多数据了
			if(!this.more){
				uni.showToast({
					title: '已加载完毕',
					duration: 1000
				});
				// 直接返回，不执行下面代码了
				return;
			}
			// 正常加载下一页
			this.pageNo = this.pageNo + 1;
			uni.showLoading({
				title: '加载中'
			});
			// 不带参数的请求，就是分页加载
			this.getShares();
			setTimeout(() => {
				uni.hideLoading();
			}, 1000);
		},
		
		methods: {
			async init() {},
			async getNotice() {
				let res = await request(LATEST_NOTICE_URL, 'GET');
				console.log("通知文本："+res.data.content);
				console.log("通知状态："+res.data.showFlag);
				this.notice = res.data.content;
				this.showFlag = res.data.showFlag;
			},
			async getShares(init) {
				if(init) {
					this.pageNo = 1;
					this.more = true;
				}		
				
				let res = await request(SHARE_LIST_URL, 'GET',  {
					pageNo: this.pageNo,
					pageSize: this.pageSize
				});
				
				// 请求结束延时隐藏加载动画
				setTimeout(() => {
					uni.hideLoading();
				},2000);
				
				// 处理进入首页加载数据和下拉刷新场景
				if(init) {
					// 获得新的数据集合，将数据初始化为本次请求返回结果
					this.shares = res.data;
					// 停止下拉刷新
					uni.stopPullDownRefresh();
				} else {
					// 不是下拉刷新，是加载下一页数据，则把新数据追加到后面，不能覆盖原值
					this.shares = this.shares.concat(res.data);
				}
				// 加载到了最后一页
				if(res.data.length < this.pageSize && this.pageNo > 0) {
					this.more = false;
				}
			},
			changeTabs(evt) {
				let { index } = evt.currentTarget.dataset;
				if (index == this.tabsIndex) return;
				this.setData({
					tabsIndex: index
				});
			},
			gotoDetail(id) {
				uni.navigateTo({
					url: "/pages/detail/detail?id=" + id
				})
			},
			gotoExchange(id) {
				uni.navigateTo({
					url: "/pages/exchange/exchange?id=" + id
				})
			}
		}
	};
</script>

<style lang="scss" scoped>
	.tabs-item-title.cur {
		color: #db6055 !important;
	}
	.noticebar-clz {
		text-shadow: 1px 1px 2px #333;
		font-size: 20rpx !important;
	}
	.flex1-clz {
		margin-left: 30rpx;
		flex-shrink: 0;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		height: 120rpx !important;
		text-align: center;
		margin-right: 10rpx;
	}
	.avatar-clz {
		flex-shrink: 0;
		width: 100rpx !important;
	}
	.flex-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		height: 120rpx !important;
	}
	.text-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 500rpx !important;
		font-size: 28rpx !important;
		text-align: left;
	}
	.text1-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		font-size: 22rpx !important;
		text-align: left;
	}
	.flex3-clz {
		flex-shrink: 0;
		width: 120rpx !important;
		text-align: center;
		margin-right: 5rpx;
	}
	.text3-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.text2-clz {
		flex-shrink: 0;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.flex2-clz {
		margin-left: 10rpx;
		flex-shrink: 0;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		height: 120rpx !important;
		text-align: center;
		margin-right: 10rpx;
	}
	.avatar1-clz {
		flex-shrink: 0;
		width: 100rpx !important;
	}
	.flex4-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		height: 120rpx !important;
	}
	.text4-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 500rpx !important;
		font-size: 28rpx !important;
		text-align: left;
	}
	.text5-clz {
		flex-shrink: 0;
		width: 500rpx !important;
		font-size: 12rpx !important;
	}
	.flex5-clz {
		flex-shrink: 0;
		width: 120rpx !important;
		text-align: center;
	}
	.text6-clz {
		flex-shrink: 0;
		font-weight: bold;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.text7-clz {
		flex-shrink: 0;
		width: 140rpx !important;
		height: 50rpx !important;
	}
	.flex8-clz {
		flex-shrink: 0;
		width: 400rpx !important;
	}
	.flex6-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		text-align: left;
		margin-right: 10rpx;
	}
	.text8-clz {
		flex-shrink: 0;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 10rpx;
		width: 100rpx !important;
		font-size: 36rpx !important;
		padding-bottom: 10rpx;
		height: 100rpx !important;
		text-align: center;
		padding-right: 10rpx;
	}
	.flex7-clz {
		padding-top: 10rpx;
		flex: 1;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text9-clz {
		color: #db6055;
		font-weight: bold;
		flex: 1;
		font-size: 32rpx !important;
	}
	.line2-clz {
		flex: 1;
	}
	.container25504 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25504 {
	}
</style>
