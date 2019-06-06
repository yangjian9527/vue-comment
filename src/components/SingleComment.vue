<template>
	<div class="comment">
		<div class="avatar">
			<img :src="comment.avatar" alt="">
		</div>
		<div class="text">
			<div class="replay-detail">
				<a class="username" href="#">{{ comment.user }}</a>
				<span style="margin-left: 2.5rem" @click="clickLike(comment)">
					<span class="fa fa-thumbs-up"></span>
					<span style="margin-left:  -0.03125rem" v-if="comment.likes != 0">{{comment.likes}}</span>
					<span style="margin-left:  -0.03125rem" v-else>赞</span>
				</span>
				<!-- 	<span style="margin-left: 3rem;display: inline-block;">点赞{{comment.likes}}</span> -->
			</div>
			<!--  @click="replayItClick(comment)" -->
			<div class="replay-detail">
				<span>{{ comment.text }}</span>
			</div>
			<div class="replay-detail">
				<span class="time">03-04 11:33</span>
				<span v-if="comment.replayTotal != 0" class="replay" @click="replayClick(comment)"><span class="replay-total">{{comment.replayTotal}}
						回复</span> </span>
				<span v-else class="zero-replay" @click="replayClick(comment)">&nbsp;回复</span>
			</div>
			<comment-children :childrenVisable="childrenVisable" :commentChildren="commentChildrenComputed" @replayUser="replayItClick"></comment-children>
		</div>
	</div>
</template>

<script>
	import commentChildren from './commentChildren'
	import {
		mapGetters
	} from 'vuex'
	export default {
		name: 'singleComment',
		components: {
			commentChildren
		},
		props: ['comment', 'index'],
		data() {
			return {
				commentChildren: [],
				childrenVisable: false
			}
		},
		methods: {
			replayClick(item) {
				// 如果处于关闭状态就去数据库查数据
				if (!this.childrenVisable) {
					// 去数据库查数据
					// 演示用例
					console.log(this.index)
					// if (this.index == 0) {
					// 	this.comment.commentChildren = []
					// }
				}

				if (this.childrenVisable) {
					this.childrenVisable = false;
				} else {
					this.childrenVisable = true;
				}
				let param = {
					user: item.user,
					index: this.index,
					childrenVisable: this.childrenVisable
				}
				this.$emit("replayUser", param);
			},
			replayItClick(item) {
				let param = {
					user: item.user,
					childrenVisable: item.childrenVisable,
					index: this.index,
				}
				this.$emit("replayUser", param);
			},
			clickLike(item) {
				let param = {
					user: item.user,
					childrenVisable: item.childrenVisable,
					index: this.index,
				}
				this.$emit("clickLike", param);
			}
		},
		computed: {
			commentChildrenComputed() {
				return this.comment.commentChildren;
			}
		},
	}
</script>

<style scoped>
	/* Single-comment component */
	.comment {
		display: flex;
		padding: 0.0625rem;
		margin-bottom: 0.0625rem;
		align-items: center;
		color: #333;
		width: 6.875rem;
		background-color: #fff;
		border-radius: 0rem;
		word-break: break-all;
		font-size: 0.35rem;
		/* font-size: 0.35rem; */
		/* box-shadow: 0.0625rem 0.0625rem 0.1875rem rgba(0, 0, 0, 0.2); */
	}

	.comment .avatar {
		align-self: flex-start;
	}

	.comment .avatar>img {
		width: 0.9375rem;
		height: 0.9375rem;
		border-radius: 100%;
		align-self: start;
	}

	.comment .text {
		text-align: left;
		margin-left: 0.3125rem;
	}

	.comment .text span {
		margin-left: 0.3125rem;
	}

	.comment .text .username {
		font-size: 0.3rem;
		font-weight: bold;
		color: #333;
	}

	.comment .text .replay-detail {
		margin-bottom: 0.1875rem;
		max-width: 5rem;
	}

	.comment .text .replay-detail .time {
		font-size: 0.25rem;
	}

	.comment .text .replay-detail .replay {
		display: inline-block;
		text-align: center;
		/* padd */
		/* margin: 0.125rem 0.125rem 0.125rem 0.125rem; */
		min-width: 0.9375rem;
		background-color: gainsboro;
		border-radius: 15%;
	}

	.comment .text .replay-detail .replay-total {
		text-align: center;
		margin: 0.0625rem 0.0625rem 0.03125rem 0.03125rem;
		/* margin-right: */
	}

	.comment .text .replay-detail .zero-replay {
		text-align: center;
		/* margin: 0.125rem 0.125rem 0.125rem 0.125rem; */
		/* background-color: gainsboro; */
		/* border-radius: 15%; */
	}
</style>
