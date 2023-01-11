<template>
	<div class="container">
		{{ name }}
		<div class="articleList">
			<el-collapse>
				<el-collapse-item v-for="item in list" :key="item.id" :title="item.title">
					<nuxt-link :to="item.id">{{ item.title }}</nuxt-link>
				</el-collapse-item>
			</el-collapse>
		</div>

		<div class="custom-pagination">
			<el-pagination background layout="prev, pager, next, slot" :total="total" :current-page.sync="current"
				:page-size="size" @current-change="pageChange">
				{{current}}
			</el-pagination>
		</div>
		<div class="contact">
			如有疑问，请加微信沟通：<span>yangzhiyuan2028</span>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		// asyncData
		// 调用时机:页面组件中组件渲染之前 自动调用
		// 特点：nuxtjs会帮助我们把asyncData函数的返回值 和 data配置项返回的响应式数据做融合处理 融合起来之后一同交给
		// 当前的组件使用
		// api调用写法：async + await的形式 正常使用axios就可以
		async asyncData({
			$axios,
			query
		}) {
			// 使用axios发起get请求
			const url = 'https://yangzhiyuan.top/tp5/public/getAllArticles'
			const p = {
				page: query.page||1,
				limit: 15,
				key: ''
			}
			const {
				code,
				data
			} = await $axios.$post(url, p)
			return {
				list: data.data,
				total: data.total,
				current: data.current_page
			}
		},
		data() {
			return {
				name: 'cp',
				size: 15
			}
		},
		watchQuery: ['page'],
		methods: {
			pageChange(page) {
				this.$router.push({
					path:'/',
					query:{
						page
					}
				})
			}
		}
	}
	// 1.如何加入路由功能呢？
	// 2.如果在nuxtjs中使用状态管理工具呢？vuex
	// 3.使用nuxt.js开发的项目如何完成部署上线呢？ 依赖什么？ 客户端  客户端 服务端
</script>

<style>
	.container {
		padding: 0 200px;
	}

	.articleList {
		margin-top: 30px;
	}
	
	.custom-pagination{
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0;
		padding: 50px 0;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.contact{
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0;
		margin: auto;
		width: 1000px;
		line-height: 60px;
		text-align: center;
		font-size: 20px;
		color: #555;
	}
	.contact span{
		color: red;
	}
</style>
