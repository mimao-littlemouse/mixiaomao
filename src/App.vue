<script setup>
import { RouterLink, RouterView } from 'vue-router'

layui.use(function(){
  var layer = layui.layer;
  var util = layui.util;
  var $ = layui.$;
  // 事件
  util.on('lay-on', {
    'mobile-header-navbar-menu-button': function(){
      let element = document.querySelector("#mobileHeaderNavbarMenuContent")
      layer.open({
        title: ['<i class="layui-icon layui-icon-app" style="font-size:20px"></i>', 'color:white;cursor:default;user-select: none;background:linear-gradient(111deg, #c1b2b2, #62cced, #c1b2b2, #c1b2b2, #c1b2b2, transparent);'],
        type: 1,
        offset: 'r',
        anim: 'slideLeft', // 从右往左
        area: ['85%', '100%'],
        shade: 0.5,
        shadeClose: false,
        move: false,
        id: 'ID-MobileHeaderNavbarMenuContent',
        content: $('#mobileHeaderNavbarMenuContent'),
        success: function(layero, index, that){
          // layero 弹层的最外层元素的 jQuery 对象
          // index 弹层的索引值
          // that 弹层内部原型链中的 this 

          // 获取 关闭按钮 元素 并进行图标设置
          for(let child of layero[0].children){
            if(child.classList.contains('layui-layer-setwin')){
              let closeBtn = child.children[0]
              // 将 关闭按钮的图标 换位 向右回缩的图标
              closeBtn.classList.value = "layui-icon layui-icon-spread-left"
              // 为其添加 点击事件
              closeBtn.onclick = ()=>{
                layer.close(index);
              }
            }
          }
          // 显示 内容元素
          element.classList.remove('d-none')
          
        },
        end: function(){
          // 隐藏 内容元素
          element.classList.add('d-none')
        }
      });
    },
    // look look 
    'looklook':function(){
      layer.tips('却也想带你看看这精彩的世界', this, {
        tips: 1,
        time: 3000
      });
      var index = layer.load(0, {shade: 0.3});
      setTimeout(function(){
        layer.close(index); // 关闭 loading
        // 获取 main 元素
        let app = document.querySelector('#app')
        app.scrollBy({
          top: app.clientHeight - 50,
          behavior: "smooth",
        });
      }, 3000);
    },
    // back to top
    'back-to-top':function(e){
      let app = document.querySelector('#app')
      let backToTopElement = e[0]
      // 根据 图标 类名 判断当前滚动条 是往上滚 还是下滚
      if(backToTopElement.classList.contains('down')){
        if(backToTopElement.classList.contains('disabled')){
          layer.tips('已经到底了', this, {
            tips: 1,
            time: 1500
          });
        }else{
          app.scrollBy({
            top: app.clientHeight - 50,
            behavior: 'smooth'
          })
        }
      }else{
        if(backToTopElement.classList.contains('disabled')){
          layer.tips('可以试试另一个', this, {
            tips: 1,
            time: 1500
          });
        }else{
          app.scrollBy({
            top: (0 - app.clientHeight + 50),
            behavior: 'smooth'
          })
        }
      }
    }
  });
});
let app = document.querySelector("#app")
app.onscroll = ()=>{
  let scrollTop = app.scrollTop
  let clientHeight = app.clientHeight
  let scrollHeight = app.scrollHeight
  let backToTop = document.querySelector("#backToTop")
  let topClassList = backToTop.children[0].classList
  let downClassList = backToTop.children[1].classList
  // 如果 到底部或位于顶部，给图标 添加 disabled 类名
  if(scrollTop + clientHeight == scrollHeight){
    downClassList.add('disabled')
  }else if(scrollTop == 0){
    topClassList.add('disabled')
  }else{
    if(topClassList.contains('disabled')){
      topClassList.remove('disabled')
    }else if(downClassList.contains('disabled')){
      downClassList.remove('disabled')
    }
  }
}
</script>

<template>
  <!-- 视图宽度 >= 576px （sm）之后，显示移动端部分 -->
  <!-- 视图宽度 > 576 显示桌面端 -->
  <header style="height: 50px;" class="row">
    <!-- 导航栏 公有部分 -->
    <div class="col-4 ps-4 d-flex align-items-center user-select-none" >
      <img src="/src/assets/logo.png" alt="" style="widows: 45px;height: 45px;">
      <!-- 桌面端显示 移动端隐藏  -->
      <span class="d-none d-md-block mixiaomao-text mixiaomao-gradient">
        MiXiaoMao
      </span>
    </div>

    <!-- 桌面端 导航栏部分 d-none d-md-block-->
    <nav class="col-8 d-none d-md-flex align-items-center justify-content-end pe-3">
      <div class="row w-100 h-100">
        <!-- 左 区域 -->
        <div class="col-10 h-100 d-flex justify-content-end">
          <ul class="m-0 p-0 d-flex h-100 header-navbar-menu">
            <li class="mx-2 px-3 menu-item text-center h-100 d-flex align-items-center text-white">首页</li>
            <li class="mx-2 px-3 menu-item text-center h-100 d-flex align-items-center text-white">关于</li>
          </ul>
        </div>
        <!-- 右 区域 -->
        <div class="col-2 h-100 d-flex justify-content-end align-items-center">
          <div style="width: 60px;" class="text-center">
            <a href="https://github.com/mimao-littlemouse/mixiaomao" target="_blank">
              <i class="layui-icon layui-icon-github" style="font-size: 24px;"></i> 
            </a>
          </div>
        </div>
      </div>
    </nav>

    <!-- 移动端 导航栏部分 d-block d-md-none -->
    <!-- 中部 文字部分 -->
    <div class="col-4 d-block d-md-none d-flex justify-content-center align-items-center user-select-none">
      <span class="mixiaomao-text mixiaomao-white">MiXiaoMao</span>
    </div>
    <!-- 右侧 展开 按钮 -->
    <div lay-on="mobile-header-navbar-menu-button" class="col-4 d-block d-md-none d-flex align-items-center justify-content-end">
      <i class="layui-icon layui-icon-shrink-right text-center" style="width: 40px;height:40px;line-height: 40px;"></i>
    </div> 
  </header>
  <!-- 移动端 导航展开部分  d-none -->
  <nav id="mobileHeaderNavbarMenuContent" class="d-flex flex-column d-none" style="background: lightgray;">
    <div class="d-flex" style="height: 50px;">
      <ul class="m-0 mt-1 p-0 d-flex w-100 h-100 header-navbar-menu text-bg-light">
        <li class="mx-2 px-3 menu-item text-center h-100 d-flex align-items-center">首页</li>
        <li class="mx-2 px-3 menu-item text-center h-100 d-flex align-items-center">关于</li>
      </ul>
    </div>
    <div class="d-flex align-items-center" style="height: 50px;background-color: #d2eff9;">
      <div style="width: 60px;" class="text-center">
        <a href="https://github.com/mimao-littlemouse/mixiaomao" target="_blank">
          <i class="layui-icon layui-icon-github" style="font-size: 24px;"></i> 
        </a>
      </div>
    </div>
  </nav>
  <main class="d-flex flex-column align-items-center">
    <!-- 介绍 部分 -->
    <section style="height: calc(100vh - 50px);" class="d-flex flex-column flex-md-row justify-content-center align-items-center">
      <img class="heroImg" src="/src/assets/logo.png" alt="" style="width: 200px;">
      <div class="d-flex flex-column align-items-center">
        <h1 class="mb-1">Hello World</h1>
        <span class="mb-3">一个不太合格的程序猿</span>
        <span lay-on="looklook" class="btn btn-primary">Go 去看看</span>
      </div>
    </section>
  </main>
  <footer style="height: 60px;" class="d-flex align-items-center justify-content-center">
    MIT Licensed, Copyright © 2024 MiXiaoMao | <a href="https://beian.miit.gov.cn/" target="_blank">湘ICP备2024041531号-1</a>
  </footer>
  <!-- 回到 顶部 -->
  <div id="backToTop" class="d-block position-absolute">
    <i lay-on="back-to-top" class="layui-icon layui-icon-triangle-d top disabled"></i>
    <i lay-on="back-to-top" class="layui-icon layui-icon-triangle-d down"></i>
  </div>
</template>

<style lang="less" scoped>
header{
  width: 100vw;
  position: fixed;
  top: 0px;
  left: 0px;
  background: linear-gradient(111deg,#c1b2b2,#62cced,#c1b2b2, #c1b2b2,#c1b2b2, transparent);
  opacity: 0.9;
  .mixiaomao-text{
    font-weight: bold;
    font-size: 18px;
  }
  .mixiaomao-gradient{
    background: linear-gradient(90deg,#5f5f5f 20%, #16b777 60%, #f8f8f8 20%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .mixiaomao-white{
    background: white;
    opacity: 0.8;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
}
.header-navbar-menu{
  list-style: none;
  .menu-item:hover{
    opacity: 0.5;
    cursor: default;
  }
  .menu-item:active{
    opacity: 1;
  }
}
main>{
  .heroImg{
    animation: heroAnimation 5s linear 5s infinite alternate;
  }
  @keyframes heroAnimation {
    0%{
      transform: scale(1) rotate(0deg);
    }
    20%{
      transform: scale(0) rotate(0deg);
    }
    40%{
      transform: scale(1) rotate(360deg);
    }
    70%{
      transform: rotate(35deg);
    }
    90%{
      transform: rotate(0deg);
    }
    to{
      transform: rotate(5deg);
    }
  }
}


// 返回 顶部
#backToTop>{
  right: 20px;
  bottom: 20px;
  i{
    width: 30px;
    height: 30px;
    text-align: center;
    line-height: 30px;
    font-size: 30px;
    font-weight: bold;
    cursor: pointer;
    display: block;
    background: linear-gradient(175deg, #0a0a0a, #16b777, #f8f8f8);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  i:hover{
    opacity: 0.8;
  }
  i.disabled{
    opacity: 0.5;
  }
  i.top{
    transform: rotate(180deg);
  }
}
</style>
