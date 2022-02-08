<template>
	<view>
		<link rel="stylesheet" type="text/css" href="https://at.alicdn.com/t/font_3170138_1ddvvb1vzpf.css"/>
		<button @tap="updateFunction">{{drag?'保存':'管理'}}</button>
		<SortByDrag :data="list" :drag="drag" height="540rpx" :itemWidth="136" :itemHeight="162" @sort="handleSort">
			<template v-slot="{item, index}">
				<view style="display: flex; justify-content: center; align-items: center; flex-direction: column;">
					<image @tap="reduceFunction(index)" v-if="drag" mode="aspectFill" src="../../static/reduce.png" style="width: 40rpx; height: 40rpx; position: absolute; right: 0; top: -10rpx; z-index: 1111"></image>
					<view>
						 <i class="iconfont" :class="item.icon"></i>
					</view>
					<view style="font-size: 24rpx; width: 100%; text-align: center;">{{item.icon}}</view>
				</view>
			</template>
		</SortByDrag>
		<view style="display: flex;">
			<view v-for="(item, index) in list2" :key="index" style="position: relative; display: flex; flex-direction: column; justify-content: center; align-items: center;">
				<image @tap="addFunction(index)" v-if="drag" mode="aspectFill" src="../../static/add.png" style="width: 40rpx; height: 40rpx; position: absolute; right: 0; top: -10rpx; z-index: 1111"></image>
				<view>
					 <i class="iconfont" :class="item.icon"></i>
				</view>
				<view style="font-size: 24rpx; width: 100%; text-align: center;">{{item.icon}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	import SortByDrag from '../../components/SortByDrag.vue'
	export default {
		components: {
			SortByDrag,
		},
		data() {
			return {
				drag: false,
				list: [{ icon: "icon-drink" },
				{ icon: "icon-basket" },
				{ icon: "icon-envelope" },
				{ icon: "icon-bonsai" },
				{ icon: "icon-dragon" },
				{ icon: "icon-bowl" },
				{ icon: "icon-gate" },
				{ icon: "icon-yin-yang" },
				{ icon: "icon-fan" },
				{ icon: "icon-lantern" },
				{ icon: "icon-lantern-" },
				{ icon: "icon-fireworks" },
				{ icon: "icon-china" },],
				list2: []
			}
		},
		created() { 
		},
		methods: {
			handleSort(list) {
				console.log('list', list)
			},
			updateFunction() {
				this.drag = !this.drag;
			},
			addFunction(index) {
				this.list.push(this.list2[index])
				this.list2.splice(index, 1)
			},
			reduceFunction(index) {
				this.list2.push(this.list[index])
				this.list.splice(index, 1)
			},
		}
	}
</script>

<style>
.iconfont{
	font-size: 60rpx;
}
</style>
