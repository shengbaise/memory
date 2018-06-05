<template>
	<div class="container">
		<div class="left" v-show="!isOver">
			<ul class="blocks">
				<li class="block" v-for="(block, index) in blocks">
					<div class="block-item" v-show="block.isShowBlock">
						<div :class="{show: block.isShow, hidden: !block.isShow}" class="colorful" :style="{ 'background-color': block.color }"></div>
					  <div :class="{hidden: block.isShow, show: !block.isShow}" @click="blockClick(index)" class="gary"></div>
					</div>
				</li>
			</ul>
		</div>
		<div class="right" v-show="!isOver">
			<div class="right-item">Score</div>
			<div class="right-item score-count">{{score}}</div>
			<div class="right-item">Time</div>
			<div class="right-item right-time">{{time | fliterTime}}</div>
			<div v-show="time === 60" class="right-item right-begin" @click="init()">开始游戏</div>
		</div>
		<div class="alert" v-show="isOver">
			<div>得分：<span></span>{{score}}</div>
			<div>游戏结束</div>
			<div @click="ok()" class="ok">再来一局</div>
		</div>
	</div>
</template>

<script>
	export default {
		data () {
			return {
				blocks: [{
					color: 'red',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'red',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'yellow',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'yellow',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'green',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'green',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'blue',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'blue',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'pink',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'pink',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'black',
					isShow: false,
					isShowBlock: true
				}, {
					color: 'black',
					isShow: false,
					isShowBlock: true
				}],
				count: 0,
				firstBlock: '',
				currentBlock: '',
				isShowBlock: true,
				allCount: 0,
				score: 0,
				time: 60
			}
		},
		filters: {
			fliterTime (item) {
				if (item === 60) {
					return '1:00'
				}
				if (item > 9 && item < 60) {
					return `0:${item}`
				}
				if (item < 10) {
					return `0:0${item}`
				}
			}
		},
		computed: {
			canClick () {
				return this.count < 2
			},
			isOver () {
				return this.allCount === 12 || this.time === 0
			}
		},
		mounted () {
			// this.init()
		},
		methods: {
			reset () {
				this.blocks.sort((a, b) => { return a > b ? 1 : -1 })
				this.blocks.forEach((v) => {
					console.info(v.color)
				})
			},
			blockClick (index) {
				if (this.canClick) {
					this.count += 1
					this.blocks[index].isShow = true
					this.current = index
					if (this.count === 1) {
						this.firstBlock = this.blocks[index]
					}
					if (this.count === 2) {
						this.currentBlock = this.blocks[index]
						if (this.currentBlock.color === this.firstBlock.color) {
							setTimeout(() => {
								this.score += 160
								this.currentBlock.isShowBlock = false
								this.firstBlock.isShowBlock = false
								this.count = 0
								this.allCount += 2
							}, 1000)
						} else {
							setTimeout(() => {
								this.currentBlock.isShow = false
								this.firstBlock.isShow = false
								this.count = 0
							}, 3000)
						}
						
					}
				}
			},
			setTime() {
			  if (this.time == 0) {
			   // this.time = 60
			  } else {
			   this.time--
			   setTimeout(() => {
			    this.setTime()
			   },
			   1000)
			  }
		  },
		  ok () {
		  	this.time = 60
		  	this.allCount = 0
		  	this.score = 0
		  	this.blocks.forEach((v) => {
		  		v.isShow = false,
					v.isShowBlock = true
		  	})
		  	// this.init()
		  },
		  init () {
		  	this.reset()
			  this.setTime()
		  }
		}
	}
</script>

<style>
	.show {
		-webkit-transition: -webkit-transform 0.8s;
    transition: transform 0.8s;
    -webkit-transform-style: preserve-3d;
    transform-style: preserve-3d;
		transform: perspective(600px) rotateX(0deg);
	}
	.hidden {
		-webkit-transition: -webkit-transform 0.8s;
    transition: transform 0.8s;
    -webkit-transform-style: preserve-3d;
    transform-style: preserve-3d;
		transform: perspective(600px) rotateX(180deg);
	}
	.container {
		position: relative;
		padding: 30px;
		background-color: #888888;
		width: 600px;
		height: 350px;
		display: flex;
		justify-content: center;
		vertical-align: center;
		flex-flow: nowrap row;
	}
	.alert {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		text-align: center;
		background-color: rgba(153, 153, 153, 0.5);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.ok {
		color: red;
		cursor: pointer;
	}
	.left {
		margin-right: 30px;
		flex: 1;
		display: flex;
		align-items: center;
	}
	.blocks {
		padding: 0;
		margin: 0;
		margin-top: -10px;
		list-style: none;
		display: flex;
		flex-flow: wrap row;
		justify-content: space-between;
	}
	.block {
		margin-top: 10px;
		width: 100px;
		height: 100px;
	}
	.block-item {
		position: relative;
		width: 100px;
		height: 100px;
	}
	.colorful {
		height: 100%;
		width: 100%;
		border-radius: 10px;
		position: absolute;
		backface-visibility: hidden;
	}
	.gary {
		height: 100%;
		width: 100%;
		background-color: #999999;
		border: 3px solid #CCCCCC;
    box-sizing: border-box;
    border-radius: 10px;
    position: absolute;
    backface-visibility: hidden;
	}
	.right-time {
		color: red;
	}
	.right {
		text-align: center;
		width: 100px;
		background-color: #999999;
		border: 3px solid #CCCCCC;
    box-sizing: border-box;
    border-radius: 10px;
	}
	.right-item {
		margin-top: 15px;
	}
	.score-count {
		color: red;
	}
	.right-begin {
		margin-top: 140px;
		color: red;
		cursor: pointer;
	}
</style>
