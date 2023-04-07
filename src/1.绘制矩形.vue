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
  resolution: window.devicePixelRatio || 1
})
//将画布添加到DOM中
document.body.appendChild(app.view)

//创建一个矩形
const rectangle = new PIXI.Graphics()
//填充颜色
rectangle.beginFill(0x66ccff)
//绘制矩形
rectangle.drawRect(200, 200, 164, 64)
//结束填充
rectangle.endFill()
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
