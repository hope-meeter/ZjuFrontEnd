<template>
	<div id="frame">
		<iframe  style="width: 100%; height: 100%;" src="https://f.jiaxintang.top/forum.php"></iframe>
	</div>
</template>

<script>
	
	export default {
		data() {
			return {
				drawer: false,
			}
		},
		mounted() {
			alert("暂时只能浏览帖子，如需登录请移步导航中“网络服务”->“枝江大学图书馆”。（在尽快处理了）")
			var iframe = document.getElementById('frame');
			iframe.style.height = (document.getElementsByClassName("v-main__wrap")[0].offsetHeight - 20) + 'px';
		},
		methods: {
			drawerHandle(res) {
				console.log(res)
			},
			openDrawer() {
				this.drawer = true
			}
		}
	}
</script>

<style>
	.container{
		padding: 0 !important;
	}
</style>
