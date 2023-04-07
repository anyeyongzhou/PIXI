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

//创建一个纹理
const texture = PIXI.Texture.from('../public/wenli.webp.jpg')
//创建一个精灵
const sprite = new PIXI.Sprite(texture)
//设置精灵的位置
sprite.x = app.screen.width / 2
sprite.y = app.screen.height / 2
//设置精灵的锚点
sprite.anchor.set(0.5, 0.5)
//设置旋转
sprite.rotation = Math.PI / 4
//设置缩放
sprite.scale.set(1, 1)
//设置透明度
sprite.alpha = 0.5

app.stage.addChild(sprite)

//ticker实现动画
app.ticker.add((delta: any) => {
  sprite.rotation += 0.01 * delta
})

//为精灵添加交互事件
sprite.interactive = true
sprite.on('click', () => {
  console.log(1)
})

sprite.on('pointerenter', () => {
  sprite.alpha = 1
})

sprite.on('pointerout', () => {
  sprite.alpha = 0.5
})

// sprite.on('mouseenter', () => {
//   sprite.alpha = 1
// })

// sprite.on('mouseout', () => {
//   sprite.alpha = 0.5
// })

//创建一个矩形
const rectangle = new PIXI.Graphics()
rectangle.beginFill('0xff0000')
rectangle.drawRect(200, 200, 100, 100)
rectangle.endFill()

app.stage.addChild(rectangle)

//为矩形添加交互事件
rectangle.interactive = true
rectangle.on('click', () => {
  console.log(2)
})

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
