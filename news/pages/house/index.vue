<template>
	<view>
		<!-- <view class="head-bg">
			<image src="../../static/top-44.png"></image>
		</view> -->
		<view>
			<form @submit="formSubmit">
				<view class="uni-flex uni-row">
					<view class="title">城市:</view>
					<picker mode="region" :range="city" @change="cityChange" name="city">
						<view class="picker">{{city}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">用途:</view>
					<picker :value="usage[index]" :range="usage" @change="usageChange" name="use">
						<view class="picker">{{usage[index]}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">基准日:</view>
					<picker mode="date" :range="date" start="1900-12-31" end="2099-12-31" @change="dateChange" name="date">
						<view class="picker">{{date}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">房产位置:</view>
					<input type="text" name="addr" :value="addr" placeholder="请输入地址" />
				</view>
				<view class="uni-row">
					<map style="width: 750upx; height: 300upx;" :latitude="lat" :longitude="lon" markers="covers" @tap="openMap"></map>
				</view>
				<view class="uni-btn-v uni-common-mt">
					<button class="btn-submit" form-type="submit" type="primary">下一步</button>
				</view>


			</form>
		</view>
	</view>
</template>

<script>
	export default {
		comments: {},
		data() {
			return {
				index: 0,
				city: '北京',
				usage: ['住宅', '写字楼', '商铺'],
				date: '2020-01-01',
				addr: '',
				lat: 39.91231,
				lon: 116.36611,

				covers: [{
					lat: 39.91321,
					lon: 16.36611,
				}]
			}
		},
		methods: {
			formSubmit(e) {
				console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
				var formdata = JSON.stringify(e.detail.value)
				// uni.showModal({
				// 	content: '表单数据内容：' + JSON.stringify(formdata),
				// 	showCancel: false
				// });
				e.detail.value.lat = this.lat
				e.detail.value.lon = this.lon
				// console.log('用途是：'+e.detail.value.use)  // ? usage存在-1???  use参数会取到0
				var type = e.detail.value.use
				if (type === "住宅") {
					// console.log('跳转至0')
					uni.navigateTo({
						url: 'esf?formdata=' + formdata // url + 参数 ?  参数字符串 下一个页面onLoad获取参数

					})
				} else if (type === "写字楼") {
					// console.log('跳转至1')
					uni.navigateTo({
						url: 'office?formdata=' + formdata
					})
				} else {
					// console.log('跳转至2')
					uni.navigateTo({
						url: 'shop?formdata=' + formdata
					})
				}
			},
			cityChange(e) {
				this.city = e.detail.value // 切换城市后地图定位到新的城市
				console.log('ccccccccccccccccity' + this.city)
				// this.lat = 40.234
				// this.lon = 115.254
			},
			usageChange(e) {
				this.index = e.detail.value
			},
			dateChange(e) {
				this.date = e.detail.value
			},
			// onkeyInput(){
			// 	uni.getLocation({
			// 		type:'wgs84',
			// 		success:function(res){
			// 			console.log('经度:'+res.latitude);
			// 			console.log('纬度:'+ res.longitude);
			// 			this.lat = res.latitude;
			// 			this.lon = res.longitude;
			// 		}
			// 	})
			// },
			openMap() {
				// console.log('addr---'+this.addr);
				uni.chooseLocation({ // 该接口中获取不到data中的变量
					success: function(res) {
						try {
							uni.setStorage({
								key: "location",
								data: {
									'name': res.name,
									'address': res.address,
									'lat': res.latitude,
									'lon': res.longitude
								}
							})
							console.log('添加地址缓存')
						} catch (e) {
							console.log(e)
						}
					}
				});

				// 同步获取缓存
				try {
					var location = uni.getStorageSync('location')
					this.addr = location.address
					this.lat = location.lat
					this.lon = location.lon
					// console.log('addr====='+this.addr)
				} catch (e) {
					console.log(e)
				}
			}
		}
	}
</script>

<style>
	.head-bg {
		width: 100%;
		height: 304rpx;
	}

	.head-bg image {
		width: 100%;
		height: 304rpx;
	}

	.title {
		min-width: calc(4em + 15px);
	}

	.uni-flex {
		height: 100upx;
	}

	.btn-submit {
		background-color: #09BB07;
		margin: 200upx 50upx;
		color: #fff;

	}
</style>
