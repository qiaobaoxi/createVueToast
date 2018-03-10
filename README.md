创建一个vue插件toast

我们创建一个文件夹（任意名字）

在文件夹里面首先 npm init生成一个和创建一个src文件夹

src里面创建一个index.html

index.html body里面输入

<section class="toast-container">
    
       <div  class="toast">
       
          <span>{{message}}</span>
          
       </div>
       
</section>

style里面输入

><style>
    
>    .toast-container{
>            position: absolute;
>            left: 0;
>            top: 0;
>            bottom: 0;
>            right: 0;
>            z-index: 1000;
>            display: flex;
>            justify-content: center;
>            align-items: center;
>    } 
    
>    .toast{
>            width: 180px;
>            height: 60px;
>            line-height: 60px;
>            text-align: center;
>            background-color:rgba(0, 0, 0,0.61);
>            border-radius: 10px;
>            color:white;
>          }    
          
</style>

看下内容显示整不正确，内容在中间

让后创建lib文件夹

里面创建vue-toast.vue文件

文件内容输入

<template>
    
    <section class="toast-container">
    
       <div  class="toast">
       
          <span>{{message}}</span>
          
       </div>
       
    </section>
    
</template>

<style lang="scss">
    
    .toast-container{
    
            position: absolute;
            
            left: 0;
            
            top: 0;
            
            bottom: 0;
            
            right: 0;
            
            z-index: 1000;
            
            display: flex;
            
            justify-content: center;
            
            align-items: center;
            
        .toast{
        
            width: 180px;
            
            height: 60px;
            
            line-height: 60px;
            
            text-align: center;
            
            background-color:rgba(0, 0, 0,0.61);
            
            border-radius: 10px;
            
            color:white;
            
          }
          
    } 
        
</style>

<script >
    
  export default{
  
      data(){
      
          return{
          
              message: 'hello toast'
              
          }
          
      }
      
  }    
</script>

我们还要创建index.js作为入口

里面内容输入

import ToastCompont from './vue-toast.vue'

let Toast = {}

Toast.install = function(Vue,option){

  //参数Vue是通过vue.use传来了的 
  
  var opt = {
  
      duration:3000
      
  }
  
  //用户配置
  
  for(var key in options){
  
    opt[key] = options[key] 
    
  }
  Vue.prototype.$toast=function(){
  
      const ToastController = Vue.extend(ToastCompont)//extend 相等于继承ToastCompont里面的内容得到全新的实例
      
      var instance = new ToastController().$mount(document.createElement('div'))//把他看作new Vue()
      
      instance.message = message;
      
      instance.visible = true;
      
      document.body.appendChild(instance.$el);
      
      setTimeout(()=>{
      
          instance.visible = false;
          
          document.body.removeChild(instance.$el);//instance.$el就是挂载里面的左右的内容  })
          
  }
  
}

export default Toast

下面我们来用webpack编译这些内容

我们在最上层文件夹下创建webpack.config.js//webpack 默认配置文件名字是webpack.config.js

和创建.babelrc用来编译es6语法

 var path= require('path')

 module.exports={

    entry: './src/lib/index.js',//入口文件
    
    output:{
    
        path:path.join(__dirname,'./dist'),//输出位置
        
        filename: 'vue-toast-demo.js',//输出名字
        
        libraryTarget:'umd',//输出格式为amd，cmd，common不要只写amd或cmd或common
        
        library:'VueToastDemo'//库名字
        
     },
    module:{
    
      rules:[
      
          {
          
              test:/\.vue$/,//匹配文件
              
              loader: 'vue-loader',//用loader编译vue
              
              exclude:'/node_modules/',//排除文件夹，防止编译这里面的文件
              
              options:{
              
                  loaders:{
                  
                     //因为vue里面有scss语法格式所以有sass-loader编译成css再用css-loader编译成js再把它输入到style 从右到左编译
                     
                      scss: 'style-loader!css-loader!sass-loader'
                      
                  }
                  
              }
              
          },
          
          {
          
              test:/\.js$/,
              
              loader: 'babel-loader',
              
              exclude:'/node_modules/'
              
          }
          
      ]
      
    },
    
    plugins:[]
    
}

最后在一开始index.html内容去掉

输入

 <!DOCTYPE html>

 <html lang="en">
    
 <head>
    
    <meta charset="UTF-8">
    
    <title>Document</title>
    
    <script  src="../node_modules/vue/dist/vue.js"></script>
    
    <script  src="./../dist/vue-toast-demo.js"></script>
    
</head>

<body>
    
    <div id="app">
    
        <a href="javascript:;" @click="toast">点击</a>
        
    </div>   
    
</body>

<script>
    
   new Vue({
   
       el:'#app',
       
       methods:{
       
           toast(){
           
               this.$toast.show('你好')
               
           }
           
       }
       
   })
   
 </script>

 </html>







