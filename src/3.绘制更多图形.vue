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

//绘制一个圆形
const cicle = new PIXI.Graphics()
cicle.beginFill(0x66ccff, 0.9)
cicle.drawCircle(0, 0, 32)
cicle.position.set(100, 100)
cicle.endFill()

app.stage.addChild(cicle)

//绘制一个圆角矩形
const roundedRectangle = new PIXI.Graphics()
roundedRectangle.beginFill(0x66ccff, 0.9)
roundedRectangle.drawRoundedRect(0, 0, 164, 64, 10)
roundedRectangle.position.set(500, 500)
roundedRectangle.endFill()
app.stage.addChild(roundedRectangle)

//绘制一个椭圆
const ellipse = new PIXI.Graphics()
ellipse.beginFill(0x66ccff, 0.9)
ellipse.drawEllipse(0, 0, 164, 64)
ellipse.position.set(400, 400)
ellipse.endFill()

app.stage.addChild(ellipse)

//绘制一个多边形
const polygon = new PIXI.Graphics()
polygon.beginFill(0x66ccff, 0.9)
//每两个值代表一个坐标
polygon.drawPolygon(0, 0, 100, 0, 100, 100, 0, 100)
polygon.position.set(100, 300)
polygon.endFill()

app.stage.addChild(polygon)

//绘制一个圆弧
const arc = new PIXI.Graphics()
arc.beginFill(0x66ccff, 0.9)
arc.arc(0, 0, 32, 0, Math.PI, false)
arc.position.set(100, 500)
arc.endFill()

app.stage.addChild(arc)

//绘制一个线段
const line = new PIXI.Graphics()
line.lineStyle(4, 0xff0000, 1)
line.moveTo(0, 0)
line.lineTo(100, 100)
line.lineTo(100, 200)
line.position.set(500, 50)

app.stage.addChild(line)

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
