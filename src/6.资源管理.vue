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

//添加资源
PIXI.Assets.add('jian', '../public/mujian.webp.jpg')
PIXI.Assets.add('man', '../public/man.webp.jpg')
PIXI.Assets.add('women', '../public/women.webp.jpg')
PIXI.Assets.add('wenli', '../public/wenli.webp.jpg')

//异步加载资源
const texturePromise = PIXI.Assets.load(
  ['jian', 'man', 'women', 'wenli'],
  (progress) => {
    console.log('加载完成', progress)
  }
)

//加载后完成精灵创建
texturePromise.then((textures: any) => {
  //创建一个容器
  const container = new PIXI.Container()

  //创建一个精灵
  const sprite = new PIXI.Sprite(textures.jian)
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
  //设置精灵混合模式
  sprite.blendMode = PIXI.BLEND_MODES.ADD
  //设置精灵交互
  sprite.interactive = true
  //设置精灵鼠标样式
  //sprite.buttonMode = true
  //设置精灵鼠标事件
  sprite.on('pointerdown', () => {
    console.log('pointerdown')
  })
  //app.stage.addChild(sprite)
  container.addChild(sprite)
  //ticker实现动画
  // app.ticker.add((delta: any) => {
  //   sprite.rotation += 0.01 * delta
  // })
  const sprite2 = new PIXI.Sprite(textures.man)
  sprite2.scale.set(0.1)
  //app.stage.addChild(sprite2)
  container.addChild(sprite2)
  app.stage.addChild(container)
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
