<template>
	<movable-area :style="{width, height}" style="position: relative;">
		<movable-view direction="all" :disabled="!drag" :x="item.x" :y="item.y" class="box" :style="{width: itemWidth+'rpx', height: itemHeight+'rpx'}" style="position: absolute; top: 0; left: 0;" v-for="(item, index) in list" :key="index" @touchstart="moveIndex = index" @touchend="moveStop" @change="move($event, item, index)">
			<div>
				<slot :item="item" :index="index"></slot>
			</div>
		</movable-view>
	</movable-area>
</template>

<script>
	export default {
		name:"SortByDrag",
		props: {
			drag: {
				type: Boolean,
				default: false
			},
			data: {
				type: Array,
				default:() => {
					return []
				}
			},
			itemWidth: {
				type: Number,
				default: 0,
			},
			itemHeight: {
				type: Number,
				default: 0,
			},
			width: {
				type: String,
				default: '100vw'
			},
			height: {
				type: String,
				default: '540rpx'
			}
		},
		data() {
			return {
				scale: 1,
				scaleX: this.itemWidth,
				scaleY: this.itemHeight,
				moveIndex: null,
				list: [],
				isMove: false,
			}
		},
		created() {
			this.scale = uni.getSystemInfoSync().screenWidth / 750;
			this.scaleX = this.scaleX * this.scale;
			this.scaleY = this.scaleY * this.scale;
			this.list = this.data;
			this.renderList();
		},
		watch: {
			data() {
				this.list = this.data;
				this.renderList();
			}
		},
		methods: {
			renderList() {
				this.list.forEach((item, index)=>{
					this.$set(item, 'index', index)
					this.$set(item, 'x', index%5 * this.scaleX)
					this.$set(item, 'y',  Math.floor(index/5) * this.scaleY)
				})
			},
			moveStop(stop) {
				if(this.isMove)
				this.$nextTick(() => {
					const temp = [];
					this.list.forEach((item, index)=>{
						if(item.index || item.index === 0) {
							temp[item.index] = item
						} else {
							temp[index] = item
						}
					})
					temp.forEach((item, index)=>{
						item.x = index%5 * this.scaleX;
						item.y= Math.floor(index/5) * this.scaleY;
					})
					this.list = [];
					this.$nextTick(()=>{
						this.list = temp;
						this.$emit('sort', temp)
					})
					this.isMove = false;
				})
			},
			moving(moveIndexX, moveIndexY,i) {
				const start = i;
				let end = moveIndexY * 5 + moveIndexX + start;
				const len = this.list.length - 1;
				this.$nextTick(() => {
					this.list.forEach((item, index)=>{
						this.$set(item, 'index', index)
						this.$set(item, 'x', index%5 * this.scaleX)
						this.$set(item, 'y',  Math.floor(index/5) * this.scaleY)
						if(start === index) {
							item.index = end >= len ? len : end;
						}
						if(start > end) {
							// 前移动
							if(index >= end && index < start) {
								item.index = index + 1;
								this.$set(item, 'x', (index + 1)%5 * this.scaleX+1)
								this.$set(item, 'y',  Math.floor((index + 1)/5) * this.scaleY+1)
							}
						}
						if(start < end) {
							// 后移动
							if(index <= end && index > start) {
								item.index = index - 1;
								this.$set(item, 'x', (index - 1)%5 * this.scaleX+1)
								this.$set(item, 'y',  Math.floor((index - 1)/5) * this.scaleY+1)
							}
						}
					})
				})
				
			},
			move(e, item, index) {
				if(this.moveIndex !== index)
					return;
				this.isMove = true;
				let x = e.detail.x
				let y = e.detail.y
				const my = (y - item.y) < 0 ? Math.ceil((y - item.y)/this.scaleY) :  Math.floor((y - item.y)/this.scaleY) 
				this.moving(Math.floor((x - item.x)/this.scaleX), my, index)
			},
		}
	}
</script>

<style>

</style>
