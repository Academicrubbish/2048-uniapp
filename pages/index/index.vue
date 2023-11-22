<template>
	<view class="body">
		<view class="container">
			<view class="item" :class="item !== 0 ? 'hide':''" v-for="(item,index) in matrix" :key="index"></view>
		</view>
		<view class="container base" @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd">
			<view class="baseItem" :class="item === 0 ? '' : ('n'+item)" v-for="(item,index) in matrix" :key="index">
				{{item === 0 ? '':item}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				matrix: Array.from({
					length: 16
				}, () => 0),
				startX: 0,
				startY: 0,
				endX: 0,
				endY: 0,
			}
		},
		onLoad() {
			this.getRandomNumber()
			this.getRandomNumber()
		},
		methods: {
			getRandomNumber() {
				const emptyCells = []
				this.matrix.forEach((item, index) => {
					if (item == 0) {
						emptyCells.push(index)
					}
				})
				if (emptyCells.length != 0) {
					const randomIndex = Math.floor(Math.random() * emptyCells.length);
					this.matrix[emptyCells[randomIndex]] = Math.random() < 0.85 ? 2 : 4;
				}
			},
			moveLeft() {
				let result = [];
				for (let i = 0; i < this.matrix.length; i += 4) {
					let row = this.matrix.slice(i, i + 4);
					// 移动非零元素到数组左侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.push(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 0; i < 3; i++) {
						if (row[i] === row[i + 1]) {
							row[i] *= 2;
							row[i + 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.push(0);
					}

					result = result.concat(row);
				}
				if (JSON.stringify(this.matrix) !== JSON.stringify(result)) {
					this.matrix = result;
					this.getRandomNumber();
				}
			},
			moveRight() {
				let result = [];
				for (let i = 0; i < this.matrix.length; i += 4) {
					let row = this.matrix.slice(i, i + 4);
					// 移动非零元素到数组右侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.unshift(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 3; i > 0; i--) {
						if (row[i] === row[i - 1]) {
							row[i] *= 2;
							row[i - 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.unshift(0);
					}

					result = result.concat(row);
				}
				if (JSON.stringify(this.matrix) !== JSON.stringify(result)) {
					this.matrix = result;
					this.getRandomNumber();
				}
			},
			moveUp() {
				let newArr = []
				let resArr = []
				for (let i = 0; i < 4; i++) {
					for (let j = i; j < this.matrix.length; j += 4) {
						newArr.push(this.matrix[j]);
					}
				}

				let result = [];
				for (let i = 0; i < newArr.length; i += 4) {
					let row = newArr.slice(i, i + 4);
					// 移动非零元素到数组左侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.push(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 0; i < 3; i++) {
						if (row[i] === row[i + 1]) {
							row[i] *= 2;
							row[i + 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.push(0);
					}

					result = result.concat(row);
				}

				for (let i = 0; i < 4; i++) {
					for (let j = i; j < result.length; j += 4) {
						resArr.push(result[j]);
					}
				}

				if (JSON.stringify(this.matrix) !== JSON.stringify(resArr)) {
					this.matrix = resArr;
					this.getRandomNumber();
				}

			},
			moveDown() {
				let newArr = []
				let resArr = []
				for (let i = 0; i < 4; i++) {
					for (let j = i; j < this.matrix.length; j += 4) {
						newArr.push(this.matrix[j]);
					}
				}

				let result = [];
				for (let i = 0; i < newArr.length; i += 4) {
					let row = newArr.slice(i, i + 4);
					// 移动非零元素到数组左侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.unshift(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 3; i > 0; i--) {
						if (row[i] === row[i - 1]) {
							row[i] *= 2;
							row[i - 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.unshift(0);
					}

					result = result.concat(row);
				}

				for (let i = 0; i < 4; i++) {
					for (let j = i; j < result.length; j += 4) {
						resArr.push(result[j]);
					}
				}

				if (JSON.stringify(this.matrix) !== JSON.stringify(resArr)) {
					this.matrix = resArr;
					this.getRandomNumber();
				}
			},
			aaa(arr) {
				let result = [];
				for (let i = 0; i < arr.length; i += 4) {
					let row = arr.slice(i, i + 4);
					// 移动非零元素到数组左侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.push(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 0; i < 3; i++) {
						if (row[i] === row[i + 1]) {
							row[i] *= 2;
							row[i + 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.push(0);
					}

					result = result.concat(row);
				}
				return result
			},
			bbb() {
				let result = [];
				for (let i = 0; i < arr.length; i += 4) {
					let row = arr.slice(i, i + 4);
					// 移动非零元素到数组左侧
					row = row.filter(val => val !== 0); // 过滤掉数组中的零元素
					while (row.length < 4) {
						row.unshift(0); // 将零补齐到数组长度为4
					}

					// 合并相同元素
					for (let i = 3; i > 0; i--) {
						if (row[i] === row[i - 1]) {
							row[i] *= 2;
							row[i - 1] = 0;
						}
					}

					// 再次移动非零元素到数组左侧
					row = row.filter(val => val !== 0);
					while (row.length < 4) {
						row.unshift(0);
					}

					result = result.concat(row);
				}
				return result
			},
			handleTouchStart(e) {
				this.startX = e.touches[0].pageX;
				this.startY = e.touches[0].pageY;
			},
			handleTouchMove(e) {
				this.endX = e.touches[0].pageX;
				this.endY = e.touches[0].pageY;
			},
			handleTouchEnd() {
				const offsetX = this.endX - this.startX;
				const offsetY = this.endY - this.startY;
				if (offsetX === 0 && offsetY === 0) {
					// 用户仅点击了屏幕，未进行滑动操作
				} else if (Math.abs(offsetX) > Math.abs(offsetY)) {
					// 判断用户水平滑动方向
					if (offsetX > 0) {
						this.moveRight()
					} else {
						this.moveLeft()
					}
				} else {
					// 判断用户垂直滑动方向
					if (offsetY > 0) {
						this.moveDown()
					} else {
						this.moveUp()
					}
				}
			}
		},
	}
</script>
<style lang="scss">
	.body {
		background-color: white;
		height: 100vh;
		width: 100%;
		position: relative;
		z-index: 9;
	}

	.container {
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-template-rows: repeat(4, 1fr);
		padding: 20rpx;
		gap: 20rpx;
		width: 90%;
		aspect-ratio: 1/1;
		border-radius: 20rpx;
		background-color: rgba(185, 173, 161, 0.3);
		z-index: 99;

		.item {
			border-radius: 30rpx;
			background-color: rgb(202, 192, 180);
			font-size: 24px;
			text-align: center;
			z-index: 99;
		}
	}

	.base {
		background-color: transparent;
		z-index: 999 !important;

		.baseItem {
			border-radius: 30rpx;
			font-size: 70rpx;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #64574e;
			font-weight: bold;
			z-index: 999;
			position: relative;
			transition: top 0.15s, left 0.15s, background-color 0.15s, color 0.15s;
		}
	}

	.hide {
		background-color: transparent !important;
	}

	.n2 {
		background-color: #eee3da
	}

	.n4 {
		background-color: #efe0c8
	}

	.n8 {
		background-color: #f26179;
		color: white !important;
	}

	.n16 {
		background-color: #f59563;
		color: white !important;
	}

	.n32 {
		background-color: #ef7c2f;
		color: white !important;
	}

	.n64 {
		background-color: #f65e36;
		color: white !important;
	}

	.n128 {
		background-color: #edcf72;
		color: white !important;
	}

	.n256 {
		background-color: #edcc61;
		color: white !important;
	}

	.n512 {
		background-color: #9c0;
		color: white !important;
	}

	.n1024 {
		background-color: #3365a5;
		font-size: 50rpx !important;
		color: white !important;
	}

	.n2048 {
		background-color: #09c;
		font-size: 50rpx !important;
		color: white !important;
	}
</style>