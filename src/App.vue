<template>
	<v-app>
		<v-navigation-drawer v-model="drawer" @input="drawerHandle" id="bar" temporary app>
			<v-list-item>
				<v-list-item-content>
					<v-list-item-title style="font-size: 2em; font-weight: bold; color: white;">
						枝江大学
					</v-list-item-title>
					<v-list-item-subtitle style="color: white;">
						Zhijiang University
					</v-list-item-subtitle>
				</v-list-item-content>
			</v-list-item>

			<v-divider></v-divider>

			<v-list dense nav style="font-size: 24px;">
				<router-link to="/">
					<v-list-item link>
						<v-list-item-icon>
							<v-icon>mdi-home</v-icon>
						</v-list-item-icon>

						<v-list-item-content>
							<v-list-item-title>
								首页
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</router-link>
				<router-link to="/research">
					<v-list-item link>
						<v-list-item-icon>
							<v-icon>mdi-feature-search</v-icon>
						</v-list-item-icon>

						<v-list-item-content>
							<v-list-item-title>
								科学研究
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</router-link>
				<router-link to="/schoolMate">
					<v-list-item link>
						<v-list-item-icon>
							<v-icon>mdi-account-group</v-icon>
						</v-list-item-icon>

						<v-list-item-content>
							<v-list-item-title>
								校友会
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</router-link>
				<router-link to="/services">
					<v-list-item link>
						<v-list-item-icon>
							<v-icon>mdi-network</v-icon>
						</v-list-item-icon>
						<v-list-item-content>
							<v-list-item-title>
								网络服务
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</router-link>
				<router-link to="/about">
					<v-list-item link>
						<v-list-item-icon>
							<v-icon>mdi-account-network</v-icon>
						</v-list-item-icon>

						<v-list-item-content>
							<v-list-item-title>
								网管中心
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</router-link>
			</v-list>
		</v-navigation-drawer>

		<v-app-bar id="bar" app>
			<v-app-bar-nav-icon dark @click="openDrawer"></v-app-bar-nav-icon>
			<div>
				<span style="font-size: 2em; font-weight: bold;">枝江大学</span>
				<span>Zhijiang University</span>
			</div>
		</v-app-bar>


		<v-main>
			<v-container fluid style="padding: 5% 10%;">
				<router-view></router-view>
			</v-container>
		</v-main>

		<v-footer app>
			<span style="text-align: center;">网站使用问题和域名售卖可联系站长：me@zhijiang.university</span>
		</v-footer>
	</v-app>
</template>

<style lang="scss">
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
	}

	#nav {
		padding: 30px;

		a {
			font-weight: bold;
			color: #2c3e50;

			&.router-link-exact-active {
				color: #E799B0;
			}
		}
	}

	#bar {
		color: white;
		background-color: #E799B0;

		a {
			color: white;
			text-decoration: none;
		}
	}
</style>

<script>
	var _hmt = _hmt || [];
	(function() {
		var hm = document.createElement("script");
		hm.src = "https://hm.baidu.com/hm.js?7d1cc9098efb5ef7b72768c59d431293";
		var s = document.getElementsByTagName("script")[0];
		s.parentNode.insertBefore(hm, s);
	})();
	export default {
		data() {
			return {
				drawer: false,
			}
		},
		mounted() {
			document.title = '枝江大学';
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
