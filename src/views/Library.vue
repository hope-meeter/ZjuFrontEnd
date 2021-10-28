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
