<template>
	<view class="container">
		<view class="prompt" v-if="isShowEject">
			<eject @closeEject='closeEject' />
		</view>
		<view class="select-container">
			<view class="original_img_box">
				原图：<image :src="`http://127.0.0.1:3000/images/original_img/${imgIndex}_img_.jpg`" mode=""></image>
			</view>
			<view class="enhanced_img_box">
				<view class="enhanced_img_box_title">
					下面是增强后的图片
				</view>
				<view class="img_box" v-for="(imgPath,index) in enhancedImgList" :key='index' @click="chooseItem(index)">
					<image class="enhanced_img"  :src="imgPath" mode="" ></image>
					<image class="sequence_img" 
						v-show="selectedArr.indexOf(index)!==-1"
						:src="`../../static/c${selectedArr.indexOf(index)+1}.png`" mode=""></image>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isShowEject: false,
				imgIndex:1,
				selectedArr:[]
			}
		},
		computed:{
			enhancedImgList(){
				let imgList = [
					`http://127.0.0.1:3000/images/enhanced_img(Blurriness-based)/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（DCP）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（论文）/${this.imgIndex}_img_.png`,
					`http://127.0.0.1:3000/images/enhanced_img（GDCP）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（Histogram Prior）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（MSRCR）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（RCP）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（Retinex-based）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img(Two-step-based)/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img（UDCP）/${this.imgIndex}_img_.jpg`,
					`http://127.0.0.1:3000/images/enhanced_img(uwcnn)/${this.imgIndex}_img_.jpg`
				]
				return imgList
			}
		},
		onLoad() {
			uni.getStorage({
				key: 'isShowEject',
				success: (res) => {
					this.isShowEject = JSON.parse(res.data)
				},
				fail:()=> {
					this.isShowEject = true
				}
			});
			
			uni.getStorage({
				key: 'imgIndex',
				success: (res) => {
					this.imgIndex = JSON.parse(res.data)
				},
				fail:()=> {
					this.imgIndex = 1
				}
			});
		},
		onHide() {
			uni.setStorage({
				key: 'imgIndex',
				data:this.imgIndex
			})
		},
		methods: {
			closeEject() {
				this.isShowEject = false
				uni.setStorage({
					key: 'isShowEject',
					data: false
				});
			},
			chooseItem(index){
				let existIndex  = this.selectedArr.indexOf(index)
				if(existIndex !== -1){
					this.selectedArr.splice(existIndex,1)
				}else{
					this.selectedArr.push(index)
					if(this.selectedArr.length==3){
						setTimeout(()=>{
							// 将this.selectedArr和this.imgIndex 推送给后端记录
							this.imgIndex++
							this.selectedArr=[]
						},300)

					}
				}
			}
		},
	}
</script>

<style scoped lang="scss">
	.container {
		width: 100vw;
		height: 100vh;
		font-size: 14px;
		line-height: 24px;
		box-sizing: border-box;
		position: relative;
	}

	.prompt {
		position: absolute;
		z-index: 999;
	}
	
	.select-container{
		font-size: 25px;
		.original_img_box{
			display: flex;
			justify-content: center;
			align-items: center;
			image{
				width: 33vw;
				height: 33vw;
				
			}
		}
		.enhanced_img_box{
			display: flex;
			flex-wrap: wrap;
			justify-content: space-around;
			.enhanced_img_box_title{
				width: 100%;
				margin: 5px 0;
				text-align: center;
			}
			.img_box{
				width: 33vw;
				height: 33vw;
				position: relative;
				.enhanced_img{
					width: 100%;
					height: 100%;
				}
				.sequence_img{
					position: absolute;
					right: 0;
					bottom: 0;
					width: 100rpx;
					height: 100rpx;
				}
			}

		}
	}
</style>
