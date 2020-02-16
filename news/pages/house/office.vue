<template>
	<view>
		<view>
			<form @submit="formSubmit">
				<view class="uni-flex uni-row">
					<view class="title">等级:</view>
					<picker :value="level[index0]" :range="level" @change="levelChange" name="level">
						<view class="picker" >{{level[index0]}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">类型:</view>
					<picker :value="buildtype[index1]" :range="buildtype"  @change="buildingChange" name="buildtype">
						<view class="picker" >{{buildtype[index1]}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">楼层:</view>
					<picker :value="floors[index2]" :range="floors" @change="floorChange" name="floor">
						<view class="picker">{{floors[index2]}}</view>
					</picker>
				</view>
				<view class="uni-flex uni-row">
					<view class="title">装修程度:</view>
					<picker :value="decorations[index3]" :range="decorations" @change="decorationChange" name="decoration">
						<view class="picker">{{decorations[index3]}}</view>
					</picker>
				</view>
				<view class="uni-btn-v uni-common-mt">
					<button class="btn-submit" form-type="submit" size="primary" >开始评估</button>
				</view>
				
				
			</form>
		</view>
	</view>
</template>
<script>
	export default{
		data(){
			return{
				index0:0,
				index1:0,
				index2:0,
				index3:0,
				level:['暂无','甲级','乙级','丙级'],
				buildtype:['暂无','板楼','塔楼','板塔结合'],
				floors:['暂无','低楼层','中楼层','高楼层'],
				decorations:['暂无','毛坯','简装','中装','精装','豪装']
			}
		},
		// 接收参数
		onLoad:function(e){
			console.log("接收到的参数:"+e.formdata)
			var item = JSON.parse(e.formdata)
			// var item = e.formdata
			// 添加缓存
			try{
				uni.setStorage({
					key:'item',
					data:item
				})
				console.log('添加缓存')
			}catch(e){
				console.log(e)
			}
		},
		methods:{
			formSubmit(e){
				// 获取缓存
				try{
					uni.getStorage({
						key:'item',
						success:function(res){
							// console.log('缓存数据:'+ JSON.stringify(res.data))
							// console.log('住宅信息:'+ JSON.stringify(e.detail.value))
							console.log('估算参数:'+ JSON.stringify(Object.assign(res.data,e.detail.value)))
						}
					})
				}catch(e){
					console.log(e)
				}
				
				
			},
			levelChange(e){
				this.index0 = e.detail.value
			},
			buildingChange(e){
				this.index1 = e.detail.value
			},
			floorChange(e){
				this.index2 = e.detail.value
			},
			decorationChange(e){
				this.index3 = e.detail.value
			}
		}
	}
	
</script>

<style>
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
	.picker{
		outline: #333333;
		
	}
</style>
