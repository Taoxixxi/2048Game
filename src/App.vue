<template>
	<div id="app">
		<div class="container">
			<div class="direction">
				<button @click="begin">开始</button>
			</div>
			<div class="content">
				<div class="row-box" v-for="(item_childs,index) in items">
					<div class="small-box" v-for="(item,index) in item_childs"><span v-if="item.value != '0'">{{item.value}}</span></div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	var myVar;
	export default {
		name: 'app',
		data() {
			return {
				heng: 4,
				shu: 4,
				items: new Array(),
				num_arr: [2, 4]
			}
		},
		created: function () {
			this.init_data();
		},
		mounted: function () {
			let _this = this;
			document.onkeydown = function (e) {
				let key = window.event.keyCode;
				if (key == 38) {
					console.log("向上")
				}
				if (key == 39) {
					console.log("向右")
				}
				if (key == 40) {
					console.log("向下")
				}
				if (key == 37) {
					console.log("向左")
					_this.toLeft();
				}
			};
		},
		methods: {
			init_data: function () {
				for (let i = 0; i < this.heng; i++) { //创建4*4个方块
					this.items[i] = new Array();
					for (let j = 0; j < this.shu; j++) {
						let obj = {
							id: i.toString() + j,
							value: ''
						}
						this.items[i][j] = obj;
					}
				}
				var count = 2;
				for (let k = 0; k < count; k++) { //从4*4中随机取一个位置并赋值
					let ranNum1 = Math.floor(Math.random() * this.items.length);
					let ranNum2 = Math.floor(Math.random() * this.items.length);
					if (this.items[ranNum1][ranNum2].value == '') { //如果不为空，则证明和上一次取重复，走else，再取一次
						this.items[ranNum1][ranNum2].value = this.num_arr[Math.floor(Math.random() * this.num_arr
							.length)];
					} else {
						k--;
					}
				}
				console.log(this.items)
			},
			begin: function () {
				this.init_data();
				this.$forceUpdate();
			},
			toLeft: function () {
				for (let i = 0; i < this.items.length; i++) {
					let value = 0;
					for (let j = 0; j < this.items[i].length; j++) {
						let obj = this.items[i][j];
						let obj_after = this.items[i][j+1];

						if(j == this.items[i].length-1){
							let obj1 = {};
							obj1.id = this.items[i][0].id
							obj1.value = value;
							this.$set(this.items[i], 0, obj1);
							obj.value = '';
							this.$set(this.items[i], j, obj);
						}
						this.$forceUpdate();
					}
				}
				// myVar = setTimeout(this.makeRamNum(), 3000);
			},
			makeRamNum: function () { //移动完在随机位置生产随机数
				let ranNum1 = Math.floor(Math.random() * this.items.length);
				let ranNum2 = Math.floor(Math.random() * this.items.length);
				if (this.items[ranNum1][ranNum2].value == '') { //如果不为空，则证明和上一次取重复，走else，再取一次
					this.items[ranNum1][ranNum2].value = this.num_arr[Math.floor(Math.random() * this.num_arr.length)];
				}
			}
		}
	}
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

	.content {
		background-color: darkgray;
		width: fit-content;
		margin: auto;
		border-radius: 0.5rem;
		padding-top: 1rem;
	}

	.row-box {
		display: flex;
		flex-wrap: wrap;
		padding: 0 0.5rem;
	}

	.row-box .small-box:last-child {
		margin-right: 0;
	}

	.small-box {
		background-color: dimgrey;
		width: 5rem;
		height: 5rem;
		border-radius: 0.5rem;
		margin: 0 0.8rem 0.8rem 0;
		line-height: 5rem;
		color: #fff;
		font-size: 2rem;
		font-weight: 600;
	}

	.direction {
		margin-bottom: 1rem;
	}

	.direction button {
		font-size: 2rem;
		margin-right: 1rem;
	}
</style>