<!-- 在弹窗内使用z-paging演示(vue) -->
<template>
	<view class="content">
		<list-component />
	</view>
</template>

<script>
	import ListComponent from './ListComponent'

	export default {
		components: { ListComponent },
		data() {
			return {}
		},
		methods: {
		}
	}
</script>

<style>
	
	page {
		height: 100%;
	}
	
	.content {
		height: 100%;
	}
</style>
