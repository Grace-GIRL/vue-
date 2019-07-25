## vue课程的基础知识 ##

关于webpack 深入浅出webpack
 http://webpack.wuhaolin.cn/1%E5%85%A5%E9%97%A8/1-2%E5%B8%B8%E8%A7%81%E7%9A%84%E6%9E%84%E5%BB%BA%E5%B7%A5%E5%85%B7%E5%8F%8A%E5%AF%B9%E6%AF%94.html

2017 vue开源项目 开源项目对程序员来说是很有用的，可以提高技术水平
https://www.jianshu.com/p/be8a36626569
Element iView Best-resume-ever Vue-element-admin Vuefify
Vue-admin



##### q1 述说一下你怎么理解MVVM模式？
   mvvm是前端的分层开发思想，和后端无关。M为Model数据层,可以是通过ajax获取来的数据；V为View视图层，即html的模版结构；VM为ViewModel视图模型是核心，VM连接了M和V，提供了双向数据绑定的概念；
##### q2 v-text和v-html的区别？
   v-text不能解析数据中的标签，v-html可以解析。
##### q3 v-cloak的使用？
   差值表达式有闪烁的问题，即vue脚本未请求回来之前差值表达式无法被解析；这时候可以把vue控制的区域隐藏，等到vue脚本解析之后再显示，使用v-cloak属性；
##### q4 v-bind和v-on指令的作用？
   v-bind是属性绑定，即html上某些属性的数据需要从vue的数据层中获取，简写：，v-on是事件绑定，即`v-on:click="showMsg(1)"`简写@；
##### q5 简述v-for的使用？
   v-for即循环，可以是普通数组`v-for="(item,i) in 数组名"`,可以是对象数组`v-for="(item,index) in 对象数组名"`，也可以是对象属性`v-for="(val,key) in 对象名"`; 由于在数组前方添加，对话框的渲染问题，以及提高渲染效率的问题，可以给每一项添加唯一的标识，例如`:key="item.id"`
##### q6 v-if和v-show的区别？
   v-if 是通过`display:none`控制的，而v-show是通过删除和添加控制的；
##### q7 过滤器的使用？
   过滤器有全局过滤器和私有过滤器，过滤器的定义方法`Vue.filter(过滤器的名称，处理函数function(规定为管道符前面的值val,){})`,调用过滤器{{msg | 过滤器1的名称（参数）｜过滤器2}}
##### q8 $mount不知道什么意思？  new Vue({data:{},methods:{}}).$mount("#app");

##### q9 vue的生命周期函数？
   创建期间的生命周期函数
beforeCreat阶段data和methods尚未初始化；created阶段data和methods都可以正常访问了；
beforeMount阶段内存中页面将要挂载；mounted页面已经渲染出来，可以操作DOM元素了
   运行期间的生命周期函数--data数据被修改
beforeUpdate阶段data数据是新的，但是页面是旧的；updated阶段data数据是最新的，用户看到的界面也是最新的
   销毁期间的生命周期函数
beforeDestroy阶段实例还没有真正被销毁，data和methos还可以被使用；destroy事例已经真正被销毁了；

##### q10 关于axios的使用？ **axios.js是什么？**
 vue通过axios发起get或post请求；
 `getInfo(){ axios.get(url).then(function(res){return res.data})}`
 `postInfo( axios.post(url,{发给服务器的数据对象}).then(function(res){return res.data}))`
 将axios挂载到vue实例上 vue.prototype.$http=window.axios  **为什么要挂载？可以提升效率吗？ 因为axios上还有其他的方法，例如interceptors拦截器就是axios的方法**
 vue.prototype与vue实例的__proto__指向同一片内存空间，只要是vue构造函数new出来的实例都可以访问到$http
 `getInfo(){ this.$http.get(url).then(function(res){return res.data})}`
 `postInfo(){ this.$http.get(url,{}).then(function(res){return res.data})}`
 axios结合async,await **为什么要使用async,await**
 `async  getInfo(){ const {data} = await this.$http.get(url) }`
 `async  postInfo(){ const {data} = await this.$http.post(url,{}) }`

 ##### q11 拦截器的使用方法？
全局的拦截器
    `axios.interceptors.request.use(function(config){ vm.flag=true; return config})`		
    `axios.interceptors.request.use(function(response){ vm.flag=false; return response})`
私有的拦截器
   定义拦截器
	`interceptor(){ 
        this.$http.interseptors.request.use(function(config){ vm.flag=true; return config })
        this.$http.interseptors.request.use(function(response){ vm.flat=false; return response})`
   调用拦截器
	`created(){ this.intersepter() }`
##### q12 关于jsonp的使用？  **vue-resource.js是什么,必须放在vue.js之后？**
vue-resource发起jsonp请求
    `async jsonpInfo(){ const {body} = await this.$http.jsonp(url){ console.log(body) } }`
跨域引入jsonp？自己封装的jsonp方法去请求数据，url后设置的查询参数规定未SYSTEMCALLBACK
	`<script>
		sendJSONP('url/jsonp?callback=SYSTEMCALLBACK',function(data){console.log(data)})
    </script>`
模拟jsonp方法
    `function sendJSONP(url,callback){  
       window.SYSTEMCALLBACK=callback;
	   const scriptEle = document.createElement('script');
	   scriptEle.src=url;
	   document.body.appendChild(scriptEle);

    }`	

##### q13 请介绍一下你对动画的理解与运用？  文档上比较全，可以结合理解
使用动画类名或自定义动画类名实现动画
使用现有的动画库实现动画
使用钩子函数实现半场动画，列表动画

使用动画类名
  .v-enter .v-enter-active .v-enter-to
  .v-leave .v-leave-active .v-leave-to
使用animate.css  
  enter-active-class="fadeInDown" 
  leave-active-class="fadeOutDown"
使用钩子函数
@before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter" 
@before-leave="beforeLeave" @leave="leave" @after-leave="afterLeave"

##### q14 全局组件，私有组件，父子组件传值，ref的使用，hash监听切换组件   
          问题？关于组件中data必须是一个函数且必须返回一个对象---- 每个实例可以维护一份被返回对象的独立的拷贝：
定义全局组件
    `Vue.component('mycom',{
		template:'<div><h1 @click="showMsg">{{msg}}</h1></div>'
		data:function(){ return ｛｝ }
		methods:{},
		created(){}
	})`
定义私有组件
	`components:{
		组件的名称：｛组件的配置对象｝
	}`
父向子传值使用v-bind属性绑定机制，父向子传递方法使用v-on事件绑定机制，当子组件调用父组件传递过来的方法的时候可以传参，此时实现了子组件向父组件传值；
    `<div id="app">
		
		<mycom :pmessage="message" :pinfo="info" @func1="show(arg)">  </mycom>
		
    </div>`
    `<script>
		var vm = new Vue ({
			el:"#app";
			data:{
				message:"hello world",
			    info:{a:1,b:2} 
			},
			methods:{
				show(){
					console.log('有人调用了父节点的show方法：'+arg)
					this.msgFromSon=arg
				}
			},
			components:{
				mycom:{
					template:"<h3 @click="myclick">{{pmessage}}----{{pinfo.a}}--{{pinfo.b}}</h3>",
					props:['pmessage','pinfo'],//props只接收v-bind传递过来的数据
					data:function(){},
					methods:{
						myclick(){ this.$emit("func1","123") }
					}
				}
			}
        })

	</script>`
ref的使用
<mycom ref="mytest"></mycom>
ref可以获取页面中的元素 console.log(this.$refs.ref123.innerHTML)
ref可以获取组件中的数据和方法  this.$refs.mytest.data    this.$refs.mytest.show()

切换组件 `<component :is="comName"></component>`
通过component标签切换组件　`<a href="#home" @click="comName='home' "></a>`
通过hash值的改变来切换组件  
	`window.onhashchange=()=>{
		switch(window.location.hash){
			case '#home':
				this.comName="#home";
				break;
			case '#about':
				this.comName="#about";
				break;
			case '#movie':
				this.comName="#movie";
				break;			
		}
	}`


##### q15 关于路由的问题？ （规模化：路由，状态管理，服务端渲染）
 依赖与Vue-router  普通路由，js编程式路由

 watch监听和computed监听的区别和使用场景？


##### q16 关于webpack构建工具？

阅读 深入浅出webpack

##### q17 关于vue的项目经验



评论列表案例里面的http://39.106.32.91:3000/api/getcmtlist 无法获取？

##### q18 你对前端自动化构建工具的理解？

构建工具的用途：
代码转换、文件优化、代码分割、模块合并、代码检查等 自动化。

常用 构建工具 有如下：
npm script  
node自带功能，即 package.json 里 scripts 定义的命令。
优点是内置，但太简单。


grunt
npm script 的进化版，但集成度不高。

gulp 
grunt的加强版，引入了流的概念，可以在 插件间传递。
缺点也是集成度不高

webpack
集成度很高，社区庞大
缺点是 只能用于模块化开发的项目    模块化开发的 即使用模块化规范的入口文件
