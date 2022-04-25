# vue-team

页面布局及自适应

    //1.this相关问题：
      window.name = 'window'
      function init(){
        console.log(this.name)
      }
      function Copy(name){
        this.name = name
        console.log(this.name)
      }
      var obj={
        init:function(){
          console.log(this.name)
          var newObj = {
            text:()=>{
              console.log(this.name)
            },
            name:'newObj'
          }
          return newObj
        },
        name:'obj'
      }
      init() // 默认绑定(window)
      init.call(obj) // 显示绑定(call()或者apply()指定this的绑定对象)
      obj.init() //隐式绑定(obj中调用this指向obj)
      obj.init().text() // 箭头函数绑定(this指向外层普通函数中的this)
      var newInt = new Copy('newName')
      newInt.name //new绑定 (使用new来调用一个函数，会构造一个新对象并把这个新对象绑定到调用函数中的this。)
      优先级：
        默认<隐式<(显式||new)
        new不能和call，apply一起
        new>bind
    //2.原型链继承
      function Father() {
        this.name = 'Father'
      }
      Father.prototype.getYouName = function() {
        console.log('Father')
      }
      function Son() {
        this.name = 'Son'
      }
      // Son原型替换为Father实例
      Son.prototype = new Father()
      
      const newSon = new Son()
      newSon.getYouName() // Father
    //3.vue路由拦截
      //第一种：
      router.beforeEach((to, from, next) => {
          var isPower //判断是否有权限
          if (!isPower) {
              next('/tologinIn');
          } else if (isPower) {
              next()
          } else {
              next();
          }
      //第二种
      router.addRoutes()  //只添加拥有权限的路由表
    //4.浏览器缓存
      强缓存：
        例，Cache-control: max-age=2592000 判断过期时间是否在有效期内，在的话直接拿缓存不请求服务器
      协商缓存：
        Last-Modified & If-Modified-Since
        请求服务器，服务器会将 If-Modified-Since 的值与 Last-Modified 字段进行对比。如果相等，则表示未修改，响应 304；反之，则表示修改了，响应 200 状态码，并返回数据。
        缺点是 如果资源更新的速度是秒以下单位，那么该缓存是不能被使用的，因为它的时间单位最低是秒。
              如果文件是通过服务器动态生成的，那么该方法的更新时间永远是生成的时间，尽管文件可能没有变化，所以起不到缓存的作用。
        解决上面缺点方案：
        Etag & If-None-Match
