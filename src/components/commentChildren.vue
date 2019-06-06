<template>
	<div class="children">
		<div v-show="childrenVisable" class="comment" v-for="(comment,index) in commentChildren">
			<div class="chidlrentext">
				<div class="replay-detail username">
					{{ comment.fromuser }}<span style="font-weight: bold;">&nbsp;&nbsp; 回复&nbsp;&nbsp; </span>{{ comment.touser }} :
				</div>
				<div class="replay-detail">
					<span class="replay" @click="replayItClick(comment)">{{ comment.text }}</span>
				</div>
				<div class="replay-detail time">
					<span >04-02 20:11</span> 
				</div>
			</div>
		</div>
		</div>

</template>

<script>
	export default {
		name: "",
		props: ['commentChildren', 'childrenVisable'],
		methods: {
			replayItClick(item) {
				console.log(item);
				let param = {
					user: item.fromuser,
					index: this.index,
					childrenVisable:this.childrenVisable,
				}
				this.$emit("replayUser", param);
			},
		},
	}
</script>

<style>
	.children .comment {
		display: flex;
		padding: 0.0625rem;
		margin-bottom: 0.0625rem;
		align-items: center;
		color: #333;
		background-color: #fff;
		max-width: 5rem;
		border-radius: 10%;
		border-bottom: #EAEAEA 0.03125rem solid;
		
		/* box-shadow: 0.0625rem 0.0625rem 0.1875rem rgba(0, 0, 0, 0.2); */
	}

	.children .comment .avatar {
		align-self: flex-start;
	}

	.children .comment .avatar>img {
		width: 0.9375rem;
		height: 0.9375rem;
		border-radius: 100%;
		align-self: start;
	}

	.children .comment .chidlrentext {
		font-size: 0.3125rem;
		text-align: left;
		margin-left: 0.0625rem;
	}

	.children .comment .chidlrentext span {
		margin-left: 0.0625rem;
	}

	.children .comment .chidlrentext .username {
		font-size: 0.3rem;
		font-weight: 400;
		color: #333;
	}.children .comment .chidlrentext .time {
		font-size: 0.2rem;
	}

	.children .comment .chidlrentext .replay-detail {
		margin-bottom: 0.0625rem;
	}

	.children .comment .chidlrentext .replay-detail .replay {
		max-width: 5rem;
	}
</style>
