<template>
	<div class="hello">
		<mavon-editor ref="md" @imgAdd="imgAdd" v-model="value"></mavon-editor>
	</div>
</template>

<script>
	import axios from "axios"
	import { mavonEditor } from 'mavon-editor'
	import 'mavon-editor/dist/css/index.css'

	export default {
		components: {
			mavonEditor
		},
		name: 'HelloWorld',
		data() {
			return {
				value: ""
			}
		},
		methods: {
			// 绑定@imgAdd event
			imgAdd(pos, $file) {
				var _this = this;
				// 第一步.将图片上传到服务器.
				var formdata = new FormData(); 
				formdata.append('image', $file);
				axios({
					url: 'http://blog.com/blog/file/uploadImg',
					method: 'post',
					data: formdata,
					headers: {
						'Content-Type': 'multipart/form-data'
					},
				}).then((url) => {
					debugger
					// 第二步.将返回的url替换到文本原位置![...](./0) -> ![...](url)
					/**
					 * $vm 指为mavonEditor实例，可以通过如下两种方式获取
					 * 1. 通过引入对象获取: `import {mavonEditor} from ...` 等方式引入后，`$vm`为`mavonEditor`
					 * 2. 通过$refs获取: html声明ref : `<mavon-editor ref=md ></mavon-editor>，`$vm`为 `this.$refs.md`
					 */
					_this.$refs.md.$img2Url(pos, url.data);
				})
			}
		},
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>