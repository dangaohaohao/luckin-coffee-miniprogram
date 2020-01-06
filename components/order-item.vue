<template>
	<view class="order-item">
		<view class="order-left">
			<text :class="{'title': true, 'titleWeight': isTitleWeight}">{{title}}</text>
			<text class="desc">{{desc}}</text>
		</view>
		<view class="order-right">
			<slot />
		</view>
	</view>
</template>

<script>
export default {
	name: 'order-item',
	props: {
		title: String,
		desc: String,
		isTitleWeight: {
			type: Boolean,
			default: false
		}
	}
}
</script>

<style lang="scss" scoped>
.order-item {
	width: 100%;
	height: 70px;
	box-sizing: border-box;
	padding: 14px 0;
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	border-bottom: 1px solid #ddd;
	
	.order-left {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		
		.title {
			font-size: 14px;
		}
		.titleWeight {
			font-weight: bold;
		}
		.desc {
			font-size: 12px;
			color: #808080;
		}
	}
	.order-right {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
}
</style>
