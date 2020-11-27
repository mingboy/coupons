<template>
	<view class="container">
		<v-tabs v-model="current" :tabs="tabs" @change="changeTab" class="tab"></v-tabs>
		<view class="coupon" ref="coupon">
			<view class="item" v-for="(v, i) in couponList" @click="toCoupon(i)" :key="i">
				<view class="top">
					<view class="left">
						<view class="content">
							<image :src="v.icon" class="icon" mode="widthFix" />
							<view class="name">{{ v.name }}</view>
						</view>
						<view class="text" v-if="v.type == 1">天天可领</view>
						<view class="text" v-else-if="v.type == 2">限时秒杀</view>
					</view>
					<view class="right">免费领取</view>
				</view>
				<view class="bottom"><image :src="v.bannerPic" mode="widthFix" /></view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			current: 0,
			tabs: [
				{
					icon: '/static/all.png',
					text: '全部',
					tabId: 0,
				},
				{
					icon: '/static/ele.png',
					text: '饿了么',
					tabId: 1,
				},
				{
					icon: '/static/meituan.png',
					text: '美团',
					tabId: 2,
				},
				{
					icon: '/static/wx.png',
					text: '拓展中...',
					tabId: 5,
				}
				// {
				// 	icon: '/static/coupon/jd.png',
				// 	text: '京东',
				// 	tabId: 3,
				// },
				// {
				// 	icon: '/static/vip.png',
				// 	text: 'VIP会员',
				// 	tabId: 4,
				// }
			],
			couponList: [],
			coupons: [
				{
					name: '饿了么红包',
					icon: '/static/coupon/ele.png',
					bannerPic: '/static/coupon/ele_banner.png',
					url: 'https://s.click.ele.me/mmCKotu',
					type: 1,
					tabId: 1,
					minapp: {
						appid: 'wxece3a9a4c82f58c9',
						path: 'pages/sharePid/web/index?scene=https://s.click.ele.me/mmCKotu'
					}
				},
				{
					name: '美团外卖红包',
					icon: '/static/coupon/meituan.png',
					bannerPic: '/static/coupon/meituan_banner.png',
					url:'https://click.dingdanxia.com/mt.php?t=1&u=6424&k=yumeituan',
					type: 1,
					tabId: 2,
					minapp: {
						appid: 'wxde8ac0a21135c07d',
						path: '/index/pages/h5/h5?weburl=https%3A%2F%2Frunion.meituan.com%2Furl%3Fkey%3Da144a9fa40a42c55af00214bb4bb3993971%26url%3Dhttps%253A%252F%252Fi.meituan.com%252Fawp%252Fhfe%252Fblock%252Fa13b87919a9ace9cfab4%252F89400%252Findex.html%253Fappkey%253Da144a9fa40a42c55af00214bb4bb3993971%253A6424dingdanxiayumeituan%26sid%3D6424dingdanxiayumeituan'
					}
				},
				// {
				// 	name: '爱奇艺会员',
				// 	icon: '/static/coupon/vip.png',
				// 	bannerPic: '/static/coupon/vip_banner.png',
				// 	url:'https://p.pinduoduo.com/VJ7bHo5d',
				// 	type: 2,
				// 	tabId: 4
				// },
				// {
				// 	name: '良品铺子',
				// 	icon: '/static/coupon/jd.png',
				// 	bannerPic: '/static/coupon/sanzhisongshu.png',
				// 	url:'https://u.jd.com/tRUDjMf',
				// 	type: 2,
				// 	tabId: 3
				// },
				// {
				// 	name: '饿了么果蔬',
				// 	icon: '/static/coupon/ele.png',
				// 	bannerPic: '/static/coupon/ele_guosu.png',
				// 	url:'https://s.click.ele.me/RpRFhvu',
				// 	type: 1,
				// 	tabId: 1,
				// 	minapp: {
				// 		appid: 'wxece3a9a4c82f58c9',
				// 		path: 'pages/sharePid/web/index?scene=https://s.click.ele.me/I4Yacuu'
				// 	}
				// },
				// {
				// 	name: '抽红包立减',
				// 	icon: '/static/11.png',
				// 	bannerPic: '/static/coupon/11.jpg',
				// 	url:'https://s.click.taobao.com/Gcs9vuu',
				// 	type: 2,
				// 	tabId: 5
				// },
			]
		};
	},
	onLoad(e) {
		//#ifdef H5
		let tabId = this.$route.query.tabId ? parseInt(this.$route.query.tabId) : 0
		//#endif
		//#ifdef MP-WEIXIN
		let tabId = e.tabId ? parseInt(e.tabId) : 0
		//#endif
		for(let i in this.tabs){
			if(tabId == this.tabs[i].tabId){
				this.current = parseInt(i)
			}
		}
		this.changeTab(this.current)
	},
	onShareAppMessage(res) {
		var messages = [{
			title: '美团饿了么大额红包，每日可领！',
			path: '/pages/index/index'
		},{
			title: '吃了这么多年外卖，你知道这个秘密吗？',
			path: '/pages/index/index'
		}];
		return messages[Math.floor(Math.random()*messages.length)];
	},
	methods: {
		changeTab(index) {
			console.log('当前选中的项：' + index);
			this.couponList = []
			uni.showLoading({
			    title: '获取优惠中'
			});
			if(index == 0){
				this.couponList = this.coupons
			}else{
				for(let i in this.coupons){
					if(this.coupons[i].tabId == this.tabs[index].tabId){
						this.couponList.push(this.coupons[i])
					}
				}
			}
			//#ifdef H5
			this.$nextTick(() => {
				this.$refs.coupon.scrollTop= 0;
			})
			//#endif
			setTimeout(() => {
				uni.hideLoading()
			}, 500)
		},
		toCoupon(i){
			console.log(this.couponList[i])
			//h5
			//#ifdef H5
			window.location.href = this.couponList[i].url
			//#endif
			//微信小程序
			//#ifdef MP-WEIXIN
			if(this.couponList[i].minapp){
				wx.navigateToMiniProgram({
				  appId: this.couponList[i].minapp.appid,
				  path: this.couponList[i].minapp.path,
				  success(res) {
					// 打开成功
				  }
				})
			}
			//#endif
		}
	}
};
</script>

<style lang="scss">
page {
	background-color: #f8f8f8;
}
.container {
	font-size: 14px;
	line-height: 24px;
	position: relative;
	.tab {
		position: fixed;
		top: var(--window-top);
		left: 0;
		z-index: 9999;
	}
	.coupon {
		padding-top: 200rpx;
		.item {
			background-color: #ffffff;
			margin: 30rpx;
			border-radius: 10rpx;
			padding: 0 30rpx 30rpx 30rpx;
			.top {
				height: 116rpx;
				display: flex;
				align-items: center;
				justify-content: space-between;
				.left {
					height: 116rpx;
					width: 400rpx;
					display: flex;
					align-items: center;
					justify-content: space-between;
					.content {
						width: 100%;
					}
					.icon {
						display: inline-block;
						vertical-align: bottom;
						width: 52rpx;
						height: auto;
					}
					.name {
						text-align: left;
						display: inline-block;
						vertical-align: bottom;
						font-size: 34rpx;
						color: #000;
						line-height: 50rpx;
						font-weight: bold;
						margin-left: 15rpx;
					}
					.text {
						width: 150rpx;
						height: 38rpx;
						line-height: 38rpx;
						text-align: center;
						font-size: 24rpx;
						color: #61300e;
						background: linear-gradient(90deg, #f9db8d, #f8d98a);
						border-radius: 6rpx;
					}
				}

				.right {
					width: 170rpx;
					height: 60rpx;
					border-radius: 30rpx;
					background: linear-gradient(90deg, #ec6f43, #ea4a36);
					color: #fff;
					font-size: 28rpx;
					line-height: 60rpx;
					text-align: center;
				}
			}

			.bottom {
				height: auto;
				width: 100%;
				image {
					display: block;
					width: 100%;
					height: auto;
				}
			}
		}
	}
}
</style>
