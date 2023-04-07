<template>
  <div></div>
</template>

<script setup lang="ts">
import { onMounted, ref, reactive } from 'vue'
//导入pixi.js
import * as PIXI from 'pixi.js'
import { Circle } from 'pixi.js'
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

//显示文字
const text = new PIXI.Text('Hello World', {
  fontFamily: 'Arial',
  fontSize: 80,
  fill: 0xff0000,
  align: 'center',
  dropShadow: true,
  dropShadowColor: '#000000',
  dropShadowBlur: 4,
  dropShadowAngle: Math.PI / 2,
  dropShadowDistance: 2
})

text.x = app.screen.width / 2
text.y = app.screen.height / 2
//设置文字的锚点
text.anchor.set(0.5, 0.5)

//app.stage.addChild(text)

//绘制一个圆形
const cicle = new PIXI.Graphics()
cicle.beginFill(0x66ccff, 0.9)
cicle.drawCircle(app.screen.width / 2, app.screen.height / 2, 32)
cicle.endFill()

//app.stage.addChild(cicle)

const mujian = PIXI.Sprite.from('../public/mujian1.jpg')

//设置遮罩：设置精灵
const bunny = PIXI.Sprite.from('../public/zhezhao.webp.jpg')
//精灵铺满整个屏幕
bunny.width = app.screen.width
bunny.height = app.screen.height
//使用文字作为精灵的遮罩
//bunny.mask = text
//使用圆作为精灵的遮罩
bunny.mask = cicle
//使用图片作为精灵的遮罩
//bunny.mask = mujian

app.stage.addChild(bunny)

onMounted(() => {})
</script>
<style lang="less" scoped></style>
