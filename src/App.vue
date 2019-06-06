<template>
	<div>
		<div @click="replay">回复</div>
		<comments-vue :replayVisable="replayVisable" @closeReplay="closeReplay" :caseid="caseid"></comments-vue>
	</div>
</template>

<script>
	import commentsVue from './components/Comments.vue'

	export default {
		name: "app",
		components: {
			commentsVue
		},
		data() {
			return {
				caseid: "111",
				replayVisable: false,
			}
		},
		methods: {

			replay() {
				if (this.replayVisable) {
					this.replayVisable = false;
				} else {
					this.replayVisable = true;
				}
			},
			closeReplay() {
				this.replayVisable = false;
			}
		},
	}
</script>

<style>
	html {font-size: 40px}

	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

	.box {
		position: absolute;
		bottom: 0;
		width: 100%;
		left: 0;
	}

	.commentBox {
		padding: 1.25rem;
	}

	#display {
		height: 31.25rem;
		overflow: auto;
	}

	.replay-detail {
		height: 100%;
		overflow: auto;
	}

	.replay-name {
		color: #0069D9;
	}

	.fix-image {
		margin-top: 0.5rem;
	}
</style>
