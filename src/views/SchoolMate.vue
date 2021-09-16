<template>
	<div>
		<v-dialog v-if="addRecordDialog" v-model="addRecordDialog" max-width="600px">
			<v-card>
				<v-card-title>
					<span class="text-h5">加入校友会</span>
				</v-card-title>
				<v-card-text>
					<v-container>
						<v-row>
							<span>站长不定期对广告、明显不实信息及垃圾信息进行清除！</span>
							<span style="color: red;">请不要填入emoji表情，会发生报错！</span>
							<v-col cols="12">
								<v-text-field v-model="record.nickName" label="昵称*" required></v-text-field>
							</v-col>
							<v-col cols="12">
								<v-combobox v-model="record.schoolName" :items="schoolList" label="现就读学校*(可自动补全)"
									required>
								</v-combobox>
							</v-col>
							<v-col cols="12">
								<v-text-field v-model="record.text" label="个性签名*" required>
								</v-text-field>
							</v-col>
							<v-col cols="12" sm="6">
								<v-combobox v-model="record.contactMethod"
									:items="['QQ', '微信', '电子邮箱', '豆瓣', '贴吧', '微博', '其他[在这里直接输入]']" label="联系方式"
									required>
								</v-combobox>
							</v-col>
							<v-col cols="12" sm="6">
								<v-text-field v-model="record.contactContent" label="ID" required></v-text-field>
							</v-col>
							<v-col cols="12">
								<v-text-field v-model="record.password" label="口令*(至少1字符的任意字符串,用于辨认身份,请牢记)" required>
								</v-text-field>
							</v-col>
							<v-col cols="12">
								<vue-hcaptcha @verify="onVerify" style="text-align: left;"
									sitekey="0b0fa1c5-e3bf-4148-9e65-03aa87f72433">
								</vue-hcaptcha>
							</v-col>
							<span style="color: red;">
								点击添加即视为您知晓并同意网站
								<router-link to="/about#privacy">《隐私数据说明》</router-link>
							</span>
						</v-row>
					</v-container>
				</v-card-text>
				<v-card-actions>
					<v-spacer></v-spacer>
					<v-btn color="#E799B0" text @click="addRecordDialog = false">
						关闭
					</v-btn>
					<v-btn color="#E799B0" text @click="addRecord()">
						添加
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>
		<v-dialog v-if="getContactInformationDialog" v-model="getContactInformationDialog" max-width="600px">
			<v-card>
				<v-card-title>
					<span class="text-h5">获取联系方式</span>
				</v-card-title>
				<v-card-text>
					<v-container>
						<v-row>
							<span>完成验证码后，点击获取</span>
							<v-col cols="12" sm="9">
								<vue-hcaptcha @verify="onVerify" style="text-align: left;"
									sitekey="0b0fa1c5-e3bf-4148-9e65-03aa87f72433">
								</vue-hcaptcha>
							</v-col>
							<v-col cols="12" sm="3">
								<v-btn @click="getContactInformation()">获取</v-btn>
							</v-col>
							<v-col cols="12" sm="6">
								<v-combobox v-model="record.contactMethod"
									:items="['QQ', '微信', '电子邮箱', '豆瓣', '贴吧', '微博', '其他[在这里直接输入]']" label="联系方式">
								</v-combobox>
							</v-col>
							<v-col cols="12" sm="6">
								<v-text-field v-model="record.contactContent" label="ID*" required></v-text-field>
							</v-col>
						</v-row>
					</v-container>
				</v-card-text>
				<v-card-actions>
					<v-spacer></v-spacer>
					<v-btn color="#E799B0" text @click="getContactInformationDialog = false">
						关闭
					</v-btn>
				</v-card-actions>
			</v-card>
		</v-dialog>

		<h1 style="text-align: left;">校友会
			<v-btn dark @click="openAddDialog()" color="#E799B0">
				加入
			</v-btn>
		</h1>
		<v-data-table :search="search" :headers="headers" :items="contents" :items-per-page="10" class="elevation-1">
			<template v-slot:top>
				<v-text-field v-model="search" label="搜索" class="mx-4"></v-text-field>
			</template>
			<template v-slot:item.operation="{ item }">
				<v-btn v-if="item.flag" dark color="#999999">无联系方式</v-btn>
				<v-btn v-else dark color="#E799B0" @click="getContactInformationId = item.id;openGetContactDialog();">
					获取联系方式
				</v-btn>
			</template>
		</v-data-table>
	</div>
</template>

<script>
	import Axios from 'axios';
	import VueHcaptcha from '@hcaptcha/vue-hcaptcha';
	import {
		schoolList
	} from '/src/utils/schoolList';
	export default {
		components: {
			VueHcaptcha
		},
		data() {
			return {
				code: "",
				search: "",
				addRecordDialog: false,
				getContactInformationId: 0,
				getContactInformationDialog: false,
				record: {
					"id": null,
					"nickName": null,
					"schoolName": null,
					"text": null,
					"contactMethod": null,
					"contactContent": null
				},
				headers: [{
						text: '学号',
						align: 'start',
						sortable: false,
						value: 'id',
					},
					{
						text: '昵称',
						value: 'nickName'
					},
					{
						text: '现就读学校',
						value: 'schoolName'
					},
					{
						text: '个性签名',
						value: 'text'
					},
					{
						text: '操作',
						value: 'operation'
					}
				],
				contents: [{
					id: '0',
					nickName: 'lily',
					schoolName: '枝江大学',
					text: '恩仇趁年华轻剑快马'
				}],
				schoolList: schoolList,
			}
		},
		mounted() {
			this.getSchoolMates()
		},
		methods: {
			onVerify(res) {
				this.code = res;
			},
			openAddDialog() {
				this.addRecordDialog = true;
				this.record = {
					"id": null,
					"nickName": null,
					"schoolName": null,
					"text": null,
					"contactMethod": null,
					"contactContent": null
				};
			},
			openGetContactDialog() {
				this.getContactInformationDialog = true
				this.code = null;
				this.record = {
					"id": null,
					"nickName": null,
					"password": null,
					"schoolName": null,
					"text": null,
					"contactMethod": null,
					"contactContent": null
				};
			},
			addRecord() {
				if ((!this.record.nickName) || (!this.record.schoolName) ||
					(!this.record.text) || (!this.record.password)) {
					alert("请完整填写")
					return
				}
				if (this.code == '') {
					alert("验证码错误")
					return
				}
				var getListUrl = "https://university.jiaxintang.top/addSchoolMate";
				Axios.post(getListUrl,
					"&nickName=" + this.record.nickName +
					"&schoolName=" + this.record.schoolName +
					"&password=" + this.record.password +
					"&text=" + this.record.text +
					"&contactMethod=" + this.record.contactMethod +
					"&contactContent=" + this.record.contactContent +
					"&token=" + this.code).then((response) => {
					console.log(response)
					this.getSchoolMates()
					this.addRecordDialog = false;
				}).catch((error) => {
					alert(error)
					console.log(error);
				})
			},
			getSchoolMates() {
				var getListUrl = "https://university.jiaxintang.top/getSchoolMates";
				Axios.get(getListUrl).then((response) => {
					this.contents = response.data;
				}).catch((error) => {
					alert(error)
					console.log(error);
				})
			},
			getContactInformation() {
				this.getContactInformationDialog = true;
				var getListUrl = "https://university.jiaxintang.top/getSchoolMateContact";
				Axios.post(getListUrl, "id=" + this.getContactInformationId + "&token=" + this.code).then((response) => {
					if (response.data == '') {
						alert("验证码错误")
					}
					this.record.contactMethod = response.data[0].contactMethod;
					this.record.contactContent = response.data[0].contactContent;
				}).catch((error) => {
					alert(error)
					console.log(error);
				})
			}
		}
	}
</script>

<style>
</style>
