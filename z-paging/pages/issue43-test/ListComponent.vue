<template>
	<!-- 在弹窗内使用z-paging时，z-paging的父view必须确定宽高（或者z-paging本身确定宽高） -->
	<view style="height: 700rpx;width: 500rpx;">
		<!-- 设置fixed=false代表z-paging非铺满全屏，此时z-paging高度未确定，其父view或z-paging本身必须确定宽高 -->
		<z-paging ref="paging" :fixed="false" v-model="dataList" @query="queryList">
			<!-- 如果希望其他view跟着页面滚动，可以放在z-paging标签内 -->
			<view class="item" v-for="(item,index) in dataList" :key="index" :id="`z-paging-${item.title}`">
				<view class="item-title">{{item.title}}</view>
				<view class="item-detail">{{item.detail}}</view>
				<view class="item-line"></view>
			</view>
		</z-paging>
		<view class="btn" @click="onScroll">滚动到3</view>
	</view>
</template>

<script>
	export default {
		name: "ListComponent",
		data() {
			return {
				//v-model绑定的这个变量不要在分页请求结束中自己赋值！！！
				dataList: []
			};
		},
		methods: {
			onScroll() {
				this.$refs.paging.scrollIntoViewById('z-paging-3')
			},
			queryList(pageNo, pageSize) {
				//组件加载时会自动触发此方法，因此默认页面加载时会自动触发，无需手动调用
				//这里的pageNo和pageSize会自动计算好，直接传给服务器即可
				//模拟请求服务器获取分页数据，请替换成自己的网络请求
				const params = {
					pageNo: pageNo,
					pageSize: pageSize,
					type: 1
				}
				this.$request.queryList(params).then(res => {
					//将请求的结果数组传递给z-paging
					this.$refs.paging.complete(res.data.list);
				}).catch(res => {
					//如果请求失败写this.$refs.paging.complete(false);
					//注意，每次都需要在catch中写这句话很麻烦，z-paging提供了方案可以全局统一处理
					//在底层的网络请求抛出异常时，写uni.$emit('z-paging-error-emit');即可
					this.$refs.paging.complete(false);
				})
			}
		}
	}
</script>

<style>

	.item {
		position: relative;
		height: 150rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 0rpx 30rpx;
	}

	.item-detail {
		padding: 5rpx 15rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
		color: white;
		background-color: #007AFF;
	}

	.item-line {
		position: absolute;
		bottom: 0rpx;
		left: 0rpx;
		height: 1px;
		width: 100%;
		background-color: #eeeeee;
	}
	.btn {
		position: fixed;
		right: 60px;
		bottom: 200px;
		padding: 5px 8px;
		border: 1px solid #ccc;
	}
</style>