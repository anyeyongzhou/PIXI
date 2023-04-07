<template>
  <div></div>
</template>

<script setup lang="ts">
import { onMounted } from 'vue'
import { OutlineFilter, GlowFilter, ShockwaveFilter } from 'pixi-filters'
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

const texture = PIXI.Texture.from('../public/car.webp.jpg')

//创建一个精灵
const sprite = new PIXI.Sprite(texture)

sprite.width = app.screen.width
sprite.height = app.screen.height

const container = new PIXI.Container()

container.addChild(sprite)

app.stage.addChild(container)

//显示文字
const text = new PIXI.Text('Hello World', {
  fontFamily: 'Arial',
  fontSize: 140,
  fill: 0xffffff,
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

container.addChild(text)

//添加置换滤镜
const displaymentSprite = PIXI.Sprite.from('../public/zhihuantietu.jpg')
displaymentSprite.scale.set(0.5)
//设置纹理的重复模式：解决纹理太大,滤镜的图片太小
displaymentSprite.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT
const displacementFilter = new PIXI.DisplacementFilter(displaymentSprite)
container.addChild(displaymentSprite)
//container.filters = [displacementFilter]

//添加震波滤镜
const shockwaveFilter1: any = new ShockwaveFilter(
  //震波发生的位置
  [Math.random() * app.screen.width, Math.random() * app.screen.height],
  //震波参数
  {
    radius: Math.random() * 1000, //半径
    wavelength: 40, //波长
    amplitude: 40, //振幅
    speed: 200 //扩散速度
  },
  //开始时间
  0
)
const shockwaveFilter2: any = new ShockwaveFilter(
  //震波发生的位置
  [Math.random() * app.screen.width, Math.random() * app.screen.height],
  //震波参数
  {
    radius: Math.random() * 1000, //半径
    wavelength: 45, //波长
    amplitude: 80, //振幅
    speed: 200 //扩散速度
  },
  //开始时间
  0
)
const shockwaveFilter3: any = new ShockwaveFilter(
  //震波发生的位置
  [Math.random() * app.screen.width, Math.random() * app.screen.height],
  //震波参数
  {
    radius: Math.random() * 1000, //半径
    wavelength: 30, //波长
    amplitude: 10, //振幅
    speed: 200 //扩散速度
  },
  //开始时间
  0
)
container.filters = [
  displacementFilter,
  shockwaveFilter1,
  shockwaveFilter2,
  shockwaveFilter3
]

const createWave = (waveFilter: any, resetTime: any) => {
  waveFilter.time += 0.01
  if (waveFilter.time > resetTime) {
    waveFilter.time = 0
    waveFilter.center = [
      Math.random() * app.screen.width,
      Math.random() * app.screen.height
    ]
  }
}

//ticker实现动画
app.ticker.add((delta: any) => {
  displaymentSprite.x += 1
  displaymentSprite.y += 1
  createWave(shockwaveFilter1, 1)
  createWave(shockwaveFilter2, 1.2)
  createWave(shockwaveFilter3, 0.7)
})

//监听点击事件，根据位置创建震波滤镜
app.view.addEventListener('click', (e: any) => {
  console.log(e.clientX, e.clientY)
  shockwaveFilter1.center = [e.clientX, e.clientY]
  shockwaveFilter1.time = 0
})
onMounted(() => {})
</script>
<style lang="less" scoped></style>
