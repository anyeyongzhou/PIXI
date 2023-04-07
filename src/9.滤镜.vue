<template>
  <div></div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { OutlineFilter, GlowFilter } from 'pixi-filters'
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

const texture = PIXI.Texture.from('../public/mujian1.jpg')

//创建一个精灵
const sprite = new PIXI.Sprite(texture)
//设置精灵的位置
sprite.x = app.screen.width / 2
sprite.y = app.screen.height / 2
//设置精灵的锚点
sprite.anchor.set(0.5)

app.stage.addChild(sprite)

//创建模糊滤镜
const blurFilter = new PIXI.BlurFilter()
//设置模糊程度
blurFilter.blur = 20
//将模糊滤镜添加到精灵上
sprite.filters = [blurFilter]

//为精灵添加交互事件
sprite.interactive = true
sprite.on('pointerenter', () => {
  blurFilter.blur = 0
})
sprite.on('pointerout', () => {
  blurFilter.blur = 20
})

//创建轮廓滤镜
const outlineFilter = new OutlineFilter(5, 0xffff00) //5为轮廓宽度，后面为轮廓颜色
//sprite.filters = [outlineFilter]

//创建发光滤镜
const glowFilter = new GlowFilter({
  //发光的距离
  distance: 50,
  //外围发光强度
  outerStrength: 2,
  //内围发光强度
  innerStrength: 0,
  //发光颜色
  color: 0xff0000,
  //透明度
  quality: 0.5
})
sprite.filters = [outlineFilter, glowFilter]

onMounted(() => {})
</script>
<style lang="less" scoped></style>
