## 实战：函数式调用vue组件

- 正常书写vue组件

  > 在组件内部控制组件的状态(是否显示等等)

  ```vue
  <template>
  	<transition name="fade">
  		<div class="toast" v-show="isShow">
  			{{ msg }}
  		</div>
  	</transition>
  </template>
  <script>
  export default {
  	data() {
  		return {
  			isShow: false,
  			msg: "",
  			timer: null,
  		};
  	},
  	methods: {
  		showToast({ msg = "", isShow = false, time = 3000 }) {
  			this.msg = msg;
  			this.isShow = isShow;
  			this.timer && clearTimeout(this.t);
  			this.timer = setTimeout(() => {
  				this.isShow = false;
  			}, time);
  		},
  		hideToast() {
  			this.isShow = false;
  		},
  		destory() {
  			this.$destroy();
  		},
  	},
  };
  </script>
  <style lang="scss" scoped>
  .toast {
  	position: fixed;
  	top: 40%;
  	left: 50%;
  	transform: translateX(-50%);
  	background-color: rgba(0, 0, 0, 0.7);
  	color: #fff;
  	padding: 10px 20px;
  	border-radius: 5px;
  }
  .fade-enter,
  .fade-leave-to {
  	opacity: 0;
  }
  .fade-enter-active,
  .fade-leave-active {
  	transition: all 0.5s;
  }
  </style>
  
  ```

- 创建同级index.js文件

  ```javascript
  import Vue from 'vue'
  import Toast from './Toast.vue'
  let toastVue
  function createToast() {
    // 这里使用了 VUE 来构建一个 vnode
    // 值得注意的是， $mount() 函数没有填写任何的 dom 节点
    // 这样就变成了一个 未挂载 的 vnode 
    const vnode = new Vue({
      render: h => h(Toast)
    }).$mount()
    // 手动 将 生成的对应 dom 插进 body 里面
    document.body.appendChild(vnode.$el)
    // 返回当前实例的vue 对象
    // 没错，就是 $children[0]
    return vnode.$children[0]
  }
   
  export function showToast(args, callback) {
    // 为了让当前的实例 只有一个，防止占用太多内存
    if (!toastVue) {
      toastVue = createToast()
    }
    toastVue.showToast(args)
    callback && callback()
    return toastVue
  }
   
  export function hideToast() {
    if (!toastVue) return
    toastVue.hideToast()
    return toastVue
  }
   
  export function destoryToast() {
    if (!toastVue) return
    toastVue.destory()
  }
   
  ```

- 使用

  ```javascript
  showToast({
  	msg: 'test',
  	isShow: true
  })
  ```

  