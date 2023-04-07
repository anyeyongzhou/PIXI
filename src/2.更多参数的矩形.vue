<template>
  <div></div>
</template>

<script setup lang="ts">
import { onMounted, ref, reactive } from 'vue'
//导入pixi.js
import * as PIXI from 'pixi.js'
//创建应用
const app: any = new PIXI.Application({
  width: window.innerWidth,
  height: window.innerHeight,
  backgroundColor: 0x1099bb,
  //屏幕的像素比例
  resolution: window.devicePixelRatio || 1,
  //抗锯齿
  antialias: true
})
//将画布添加到DOM中
document.body.appendChild(app.view)

//创建一个矩形
const rectangle = new PIXI.Graphics()
//设置边框的样式:线宽、颜色、透明度
rectangle.lineStyle(4, 0xff0000, 1)
//填充颜色,透明度
rectangle.beginFill(0x66ccff, 0.5)
//绘制矩形
rectangle.drawRect(200, 200, 164, 64)
//结束填充
rectangle.endFill()
//图形的缩放
rectangle.scale.set(2, 2)
//图形的位移
rectangle.position.set(100, 100)
//图形的旋转
rectangle.rotation = 0.5

//将矩形添加到舞台
app.stage.addChild(rectangle)

onMounted(() => {})
</script>
<style lang="less" scoped>
.canvas {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  border: 1px solid black;
}
</style>
