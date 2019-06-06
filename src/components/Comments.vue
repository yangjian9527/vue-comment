<template>
	<div :class="commentsbody">
		<div v-show="replayVisable" class="comments-outside">
			<div class="comments-header">
				<div class="comments-stats" @click="closeReplay()">
					<span><i class="fa fa-times"></i></span>
				</div>
				<div class="post-owner">
					<!-- <div class="avatar">
					<img :src="creator.avatar" alt="">
				</div> -->
					<div class="username">
						<!-- <a href="#">@{{ creator.user }}</a> -->
						共{{commentsTotal}}条评论
					</div>
				</div>
			</div>
			<div class="comments">
				<div :class="comments_wrapper_classes">
					<single-comment v-for="(comment,index) in comments" :index="index" :comment="comment" :key="comment.id"
					 @replayUser="replayUser" @clickLike="clickLike"></single-comment>
					<div class="noReplay" v-if="comments.length== 0">还没有人评论过~</div>
				</div>
				<div class="reply">
					<div class="avatar">
						<img :src="current_user.avatar" alt="">
					</div>
					<input  ref="replayInput" type="text" v-model.trim="replayInfo.replay" class="reply--text" placeholder="请输入评论"
					 maxlength="250" required @keyup.enter="submitComment" />

					<span v-if="replayInfo.replay != ''" class="activeButton" @click.prevent="submitComment">发布</span>
					<span v-else class="reply--button">发布</span>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import singleComment from './SingleComment'
	export default {
		name: 'commentsVue',
		components: {
			singleComment
		},
		props: {
			replayVisable: {
				type: Boolean,
				default: false,
			},
			caseid: {
				type: String,
				default: '',
			},
		},
		data() {
			return {
				replayInfo: {
					replay: "",
					user: "",
					id: "",
					index: "-1",
					replay_time: "刚刚",
				},
				creator: {
					avatar: 'http://via.placeholder.com/100x100/a74848',
					user: 'exampleCreator'
				},
				current_user: {
					avatar: 'http://via.placeholder.com/100x100/a74848',
					user: 'exampler'
				},
				comments_wrapper_classes: ['custom-scrollbar', 'comments-wrapper'],
				comments: [],
				commentsTotal: 0,
				commentsbody:'comments-body',
				screenSize: document.body.clientWidth,
			}
		},
		methods: {
			submitComment() {
				// 整理回复信息数据结构
				let replayData = {};
				if (this.replayInfo.index == -1) {
					replayData = {
						id: this.comments.length + 1,
						touser: this.replayInfo.user,
						fromuser: this.current_user.user,
						avatar: this.current_user.avatar,
						user: this.current_user.user,
						text: this.replayInfo.replay,
						time: this.replayInfo.replay_time,
						commentChildren: [],
						replayTotal: 0,
						likes:0,
					}
				} else {
					replayData = {
						id: this.comments.length + 1,
						touser: this.replayInfo.user,
						fromuser: this.current_user.user,
						avatar: this.current_user.avatar,
						user: this.current_user.user,
						text: this.replayInfo.replay,
						time: this.replayInfo.replay_time,
					}
				}
				// 进行回复信息的填充
				if (this.replayInfo.index == '-1') {
					this.comments.push(replayData);
				} else {
					this.comments[this.replayInfo.index].commentChildren.push(replayData);
					this.comments[this.replayInfo.index].replayTotal = this.comments[this.replayInfo.index].replayTotal + 1;
				}
				this.commentsTotal = this.commentsTotal + 1;
				this.clearRepalyInfo();
			},
			// 关闭评论框
			closeReplay() {
				this.replayVisable = false;
				this.clearRepalyInfo();
				this.$emit("closeReplay");
			},
			replayUser(param) {
				if (param.childrenVisable) {
					this.replayInfo.user = param.user;
					this.replayInfo.index = param.index;
					this.$refs.replayInput.placeholder = "回复:" + param.user;
				} else {
					this.clearRepalyInfo();
				}
			},
			clickLike(param){
				console.log("点赞"+param);
				this.comments[param.index].likes = this.comments[param.index].likes + 1;
			},
			// 清空信息
			clearRepalyInfo() {
				this.replayInfo = {
					replay: "",
					user: "",
					id: "",
					index: "-1",
					replay_time: "刚刚",
				};
				this.$refs.replayInput.placeholder = '请输入评论'; //可以用计算属性
				console.log(this.replayInfo);
			},
		},
		mounted() {
			this.caseid = '0';
			const App = this;
			window.onresize = () =>{
				return (() => {
					window.screenWidth = document.body.clientWidth
					App.screenSize = window.screenWidth;
					
				})()
			}
		},
		watch: {
			caseid(o, n) {
				// 查库
				this.commentsTotal = 0; //查出来的值
				console.log(o, n);
				this.comments = [];
			},
			screenSize(o,n){
				console.log(o+"="+n);
				if(this.commentsbody == "comments-body"){
					this.commentsbody = "comments-body-top";
				}else{
					this.commentsbody = "comments-body";
				}
			},
		}


	}
</script>

<style scoped>
	.comments-body {
		position: absolute;
		bottom: 0rem;
		right: 0rem;
		left: 0rem;
	}
	.comments-body-top{
		position: absolute;
		bottom: 0.9rem;
		right: 0rem;
		left: 0rem;
	}

	.comments {
		margin-top: 0.0625rem;
		padding: 0.0625rem;
		padding-top: 0;
	}

	.comments-wrapper {
		max-height: 7rem;
		overflow-y: auto;
		border-bottom: gainsboro 0.01rem solid;
	}

	.custom-scrollbar::-webkit-scrollbar-track {
		/* 	-webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
		-moz-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
		box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3); */
		border-radius: 0.625rem;
		background-color: #fff;
	}

	.custom-scrollbar::-webkit-scrollbar {
		width: 0.125rem;
		background-color: gray;
	}

	.custom-scrollbar::-webkit-scrollbar-thumb {
		border-radius: 0.625rem;
		/* -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
		-moz-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
		box-shadow: inset 0 0 6px rgba(0, 0, 0, .3); */
		background-color: #555;
	}

	/* Reply component */
	.reply {
		display: flex;
		position: relative;
		align-items: center;
		background-color: #fff;
		border-radius: 30%;
		padding: 0.0625rem 0.0625rem;
		overflow: hidden;
	}

	.reply .avatar {
		position: absolute;
	}

	.reply .avatar>img {
		width: 0.75rem;
		height: 0.75rem;
		border-radius: 100%;
	}

	.reply .reply--text {
		height: 0.9375rem;
		padding: 0.3125rem 0.3125rem 0.36rem 1.25rem;
		margin-right: 0.3125rem;
		border: 0;
		color: #333;
		width: 90%;
		outline: 0;
		background-color: transparent;
		box-shadow: none;
	}

	/* .reply input.reply--text:valid {
		margin-right: 4.4375rem;
	} */

	/* .reply input.reply--text:valid+.reply--button {
		right: 0.625rem;
	} */
	.noReplay {
		text-align: center;
		line-height: 2rem;
		font-size: 0.5rem;
		font-weight: 100;
		color: gainsboro;
		display: inline-block;
		height: 2rem;
		width: 100%;
	}

	.activeButton {
		position: absolute;
		right: 0.3rem;
		/* border: 1px solid #2a629c; */
		background-color: transparent;
		color: #4876FF;
		font-size: 15px;
		line-height: 1.5;
		/* border: gainsboro 0.0625rem solid; */
		border-radius: 30%;
	}

	.reply .reply--button {
		position: absolute;
		right: 0.3rem;
		/* border: 1px solid #2a629c; */
		background-color: transparent;
		color: gainsboro;
		/* display: inline-block; */
		/* font-weight: 400; */
		/* text-align: center;
		white-space: nowrap;
		vertical-align: middle;
		-webkit-user-select: none;
		-moz-user-select: none;
		-ms-user-select: none;
		user-select: none;
		padding: 0.3125rem 0.3125rem; */
		font-size: 15px;
		line-height: 1.5;
		/* border: gainsboro 0.0625rem solid; */
		border-radius: 30%;
		/* transition: color 0.25s ease-in-out, background-color 0.25s ease-in-out, border-color 0.25s ease-in-out, box-shadow 0.25s ease-in-out, right 0.25s ease-in-out; */
		/* outline: 0; */
	}

	/* .reply .reply--button:hover {
		color: #fff;
		background-color: #2a629c;
	} */

	/* .reply .reply--button:focus,
	.reply .reply--button:active {
		box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
	} */

	/* hr {
		margin-top: 10px;
		margin-bottom: 10px;
	} */
	a {
		text-decoration: none;
	}

	hr {
		display: block;
		height: 1px;
		border: 0;
		border-top: 1px solid #ececec;
		margin: 1em 0;
		padding: 0;
	}

	.comments-outside {
		box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.3);
		margin: 0 auto;
		max-width: 9.375rem;
		background-color: #fff;
	}

	.comments-header {
		background-color: #fff;
		padding: 10px;
		align-items: center;
		display: flex;
		justify-content: space-between;
		color: #333;
		min-height: 0.9375rem;
		font-size: 0.4375rem;
		border-bottom: gainsboro 0.002rem solid;
	}

	.comments-header .comments-stats span {
		margin-left: 10px;
	}

	.post-owner {
		display: flex;
		align-items: center;
	}

	/* .post-owner .avatar>img {
		width: 30px;
		height: 30px;
		border-radius: 100%;
	} */

	.post-owner .username {
		font-weight: 300;
	}
</style>
