<template>
    <view class="content">
        <view class="uni-list">
            <view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in news" :key="index" @tap="opennews" :data-postid="item.post_id">
                <view class="uni-media-list">
                    <image class="uni-media-list-logo" :src="item.author_avatar"></image>
                    <view class="uni-media-list-body">
                        <view class="uni-media-list-text-top">{{item.title}}</view>
                        <view class="uni-media-list-text-bottom uni-ellipsis">{{item.created_at}}</view>
                    </view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
	import uniLoadMore from '../../components/uni-load-more/uni-load-more.vue'  // 加载更多(需下载组件)
	
    export default {
		
		components:{uniLoadMore},
		
        data() {
            return {
                news: []
            };
        },
        onLoad:function(){
            this.getNewsList();
        },
		//下拉刷新
		onPullDownRefresh(){
			this.getNewsList();
		},
        methods:{
            opennews(e){
                uni.navigateTo({
                    url: '../info/info?postid='+e.currentTarget.dataset.postid
                });
            },
			getNewsList(){
				uni.request({
				    url: 'https://unidemo.dcloud.net.cn/api/news',
				    method: 'GET',
				    data: {},
				    success: res => {
				        this.news = res.data;
				    },
				    fail: () => {},
				    complete: () => {}
				});
			}
        }
    }
</script>

<style>
    .uni-media-list-body{height: auto;}
    .uni-media-list-text-top{line-height: 1.6em;}
</style>