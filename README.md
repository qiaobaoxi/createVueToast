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
      
      var instace = new ToastController().$mount(document.createElement('div'))//把他看作new Vue()
      
      instace.message = message;
      
      instance.visible = true;
      
      setTimeout(()=>{
      
          instance.visible = false;
          
          document.body.removeChild(instace.$el);//instace.$el就是挂载里面的左右的内容  })
          
  }
  
}

export default Toast



