<template>
	<view class="container">
		<view class="content">
			<view class="content-text">
				请大家按照图像的增强效果按照顺序选择，你认为效果最好的三张图片（有先后顺序哈）
			</view>
			<view class="ok-btn" @click="closeEject">
				<view class="btn">
					有过图像处理经验
				</view>
				<view class="line"></view>
				<view class="btn">
					无图像处理经验
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"eject",
		data() {
			return {
				
			};
		},
		methods:{
			closeEject(){
				this.$emit('closeEject')
			}
		}
	}
</script>

<style lang="scss">
	.container{
		position: relative;
		width: 100vw;
		height: 100vh;
		background:rgba($color: #000000, $alpha: 0.2);
	}
	.content{
		z-index: 100;
		width: 75%;
		height: 75%;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		background-color:white;
		border-radius: 15px;
		overflow: hidden;
		display: flex;
		flex-direction: column;
		.content-text{
			flex: 1;
			padding: 50rpx;
		}
		.ok-btn{
			display: flex;
			align-items: center;
			height: 55px;
			background-color: pink;
			border-top: 1px solid gray;
			.line{
				height: 100%;
				width: 1rpx;
				background-color: gray;
			}
			.btn{
				flex: 1;
				justify-content: center;
				display: flex;
			}
		}
	}
</style>
