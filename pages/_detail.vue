<template>
	<div class="container">
		{{ name }}
		<div class="articleList">
			<p>{{article.title}}</p>
			<div v-html="article.content"></div>
		</div>
	</div>
</template>

<script>
	export default {
		// asyncData
		// 调用时机:页面组件中组件渲染之前 自动调用
		// 特点：nuxtjs会帮助我们把asyncData函数的返回值 和 data配置项返回的响应式数据做融合处理 融合起来之后一同交给
		// 当前的组件使用
		// api调用写法：async + await的形式 正常使用axios就可以
		async asyncData({
			$axios,
			params
		}) {
			console.log(params.detail)
			const url = 'https://yangzhiyuan.top/tp5/public/getArticleDetail'
			const obj = {
				id: params.detail
			}
			const {
				code,
				data
			} = await $axios.$post(url, obj)
			// eslint-disable-next-line no-console
			return {
				article: data
			}
		},
		data() {
			return {
				name: 'cp'
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
</style>
