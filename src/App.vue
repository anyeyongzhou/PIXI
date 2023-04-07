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
  backgroundColor: 0xffffff,
  //屏幕的像素比例
  resolution: window.devicePixelRatio || 1,
  //抗锯齿
  antialias: true
})
//将画布添加到DOM中
document.body.appendChild(app.view)
//创建容器
const container = new PIXI.Container()
//将容器添加到舞台
app.stage.addChild(container)

//添加恐龙游戏的精灵纹理
const baseTexture = PIXI.BaseTexture.from('../public/game.png')

//创建恐龙纹理
const dinoTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(38, 0, 44, 50)
)
//创建恐龙精灵
const dino = new PIXI.Sprite(dinoTexture)
//一开始并不想显示
dino.visible = false
container.addChild(dino)

//恐龙跑步动画
const runTextuers = []
for (let i = 0; i < 2; i++) {
  runTextuers.push(
    new PIXI.Texture(baseTexture, new PIXI.Rectangle(935 + i * 44, 0, 44, 50))
  )
}
const runAnimation = new PIXI.AnimatedSprite(runTextuers)
runAnimation.animationSpeed = 0.1
runAnimation.play()
runAnimation.visible = false
container.addChild(runAnimation)

//恐龙跳跃精灵
const junmTextuer = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(848, 0, 44, 50)
)
const jumpSprite = new PIXI.Sprite(junmTextuer)
jumpSprite.x = 60
jumpSprite.y = window.innerHeight - 50 - 50
jumpSprite.visible = false
container.addChild(jumpSprite)

//恐龙死亡精灵
const deathTextuer = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(1111, 0, 60, 50)
)
const deathSprite = new PIXI.Sprite(deathTextuer)
deathSprite.x = 60
deathSprite.y = window.innerHeight - 50 - 50
deathSprite.visible = false
container.addChild(deathSprite)

//地面精灵
const groundTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(0, 50, 1233, 17)
)
const groundSprite = new PIXI.TilingSprite(groundTexture)
groundSprite.width = window.innerWidth
groundSprite.height = 17
//设置地面精灵的位置
groundSprite.position.set(0, window.innerHeight - 50)
//groundSprite.visible = false
container.addChild(groundSprite)

//仙人掌精灵
const cactusTexture = new PIXI.Texture(
  baseTexture,
  new PIXI.Rectangle(332, 0, 23, 50)
)
const cactusSprite = new PIXI.Sprite(cactusTexture)
//设置仙人掌精灵的位置
cactusSprite.x = window.innerWidth / 2
cactusSprite.y = window.innerHeight - 50 - 50
//cactusSprite.visible = false
container.addChild(cactusSprite)

//开始游戏文字
let startText = new PIXI.Text('开始游戏', {
  fontFamily: 'Arial',
  fontSize: 36,
  fill: 0x333333,
  align: 'center'
})
startText.anchor.set(0.5, 0.5)
startText.x = window.innerWidth / 2
startText.y = window.innerHeight / 2
//startText.visible = false
container.addChild(startText)

//游戏得分
let score = 0
//最后得分文字
let overText = new PIXI.Text('', {
  fontFamily: 'Arial',
  fontSize: 36,
  fill: 0x333333,
  align: 'center'
})
overText.anchor.set(0.5, 0.5)
overText.x = window.innerWidth / 2
overText.y = window.innerHeight / 2
overText.visible = false
container.addChild(overText)

startText.interactive = true
startText.on('click', () => {
  playGame()
})

//游戏开始标记
let isGameing = false
//结束开始标记
let isGameover = false
//开始游戏函数
const playGame = () => {
  isGameing = true
  //开始游戏文字消失
  startText.visible = false
  //显示恐龙的跑步动画
  runAnimation.visible = true
  runAnimation.x = 60
  runAnimation.y = window.innerHeight - 50 - 50
}

//结束游戏函数
const Gameover = () => {
  isGameover = true
  runAnimation.visible = false
  deathSprite.visible = true
}

//初始化恐龙跳跃的速度
let jumpVelocity = 20
//初始化重力
let gravity = 1
//游戏循环
app.ticker.add((delta: any) => {
  if (isGameover) return
  if (isGameing) {
    //地面移动
    groundSprite.tilePosition.x -= 10
    //仙人掌移动
    cactusSprite.x -= 10
    if (cactusSprite.x < -30) {
      cactusSprite.x = window.innerWidth
      score++
    }
  }
  if (jumpSprite.visible) {
    jumpVelocity -= gravity
    jumpSprite.y -= jumpVelocity
    //每次跳跃落地后需重置jumpSprite的数据
    if (jumpSprite.y > window.innerHeight - 50 - 50) {
      jumpSprite.y = window.innerWidth - 50 - 50
      jumpVelocity = 20
      jumpSprite.x = 60
      jumpSprite.y = window.innerHeight - 50 - 50
      jumpSprite.visible = false
      runAnimation.visible = true
    }
  }
  //检测碰撞
  if (
    jumpSprite.y > cactusSprite.y - 50 &&
    jumpSprite.x + 44 > cactusSprite.x &&
    cactusSprite.x > jumpSprite.x - 44
  ) {
    Gameover()
    //显示最后得分
    overText.text = '游戏结束，最后得分为' + score.toString()
    overText.visible = true

    overText.interactive = true
    overText.on('click', () => {
      location.reload()
    })
  }
})

//点击空格恐龙跳跃
window.addEventListener('keydown', (e) => {
  if (e.code == 'Space') {
    runAnimation.visible = false
    jumpSprite.visible = true
    jumpVelocity = 20
  }
})
</script>
<style lang="less" scoped></style>
