<template lang="pug">
  .c-line
    .lineCanvas(ref='lineCanvas')
</template>


<script>

let PIXI
if(process.client) {
  PIXI = require('pixi.js')
}

export default {

  data() {
    return {
      app: null,
      el: null,
      scrollAmount: 0,
      windowSize: [
        { width: 0 },
        { height: 0 }
      ],
      myContainer: [
        { lines: null }
      ],
      bezierPoints: [
        { x: 0, y: 0 },
        { x: 0, y: 250 },
        { x: 580, y: 280 },
        { x: 340, y: 50 },
      ],
      myLine: [
        { pre: null },
        { current: null }
      ]
    }
  },

  mounted() {
    this.InitCanvas()

    window.addEventListener('wheel', (e) => {
      this.scrollAmount += e.deltaY / 1000
      this.MyAnim()
      console.log("Hello")
    })
  },

  methods: {
    InitCanvas() {

      this.windowSize.width = document.body.clientWidth
      this.windowSize.height = document.body.clientHeight

      this.app = new PIXI.Application({
        width: this.windowSize.width,
        height: this.windowSize.height,
        backgroundColor: 0x000000,
        antialias:true
      })

      this.el = this.$refs.lineCanvas
      this.el.appendChild(this.app.view)

      this.myContainer.lines = new PIXI.Container()
      this.myContainer.lines.x = 0
      this.myContainer.lines.y = 0
      this.myContainer.lines.pivot.x = 0
      this.app.stage.addChild(this.myContainer.lines)

      this.DrawLine(0)
      // this.DrawBezier()
    },

    DrawBezier() {
      const bezier = new PIXI.Graphics()

      const baseY = this.windowSize.height / 2

      const bezierPos = [
        { x: 0, y: baseY + 400 },
        { x: 500, y: baseY - 400 },
        { x: 1000, y: baseY + 400 },
        { x: 3000, y: baseY - 400 },
        { x: 400, y: baseY + 0 },
        { x: 500, y: baseY - 400 },
        { x: 600, y: baseY + 0 },
        { x: 700, y: baseY - 400 }
      ]

      bezier
        .lineStyle(3, 0xffffff)
        .bezierCurveTo(
          bezierPos[1].x,
          bezierPos[1].y,
          bezierPos[2].x,
          bezierPos[2].y,
          bezierPos[3].x,
          bezierPos[3].y
        )

      bezier.position.x = 0
      bezier.position.y = 0

      this.myContainer.lines.addChild(bezier)
    },

    DrawLine(_baseX) {

      const amount = 10

      for(let i = 0; i < amount; i++) {

        const keiSin = 200 + Math.cos(this.scrollAmount) * 200

        const startRad = i / 10 + this.scrollAmount
        const endRad = (i + 1) / 10 + this.scrollAmount

        const startSin = Math.sin(startRad) * keiSin + this.windowSize.height / 2
        const endSin = Math.sin(endRad) * keiSin + this.windowSize.height / 2

        const baseX = this.windowSize.width / amount + _baseX
        const baseY = 0

        const startX = baseX * i
        const endX = baseX * (i + 1)

        const startY = startSin
        const endY = endSin

        this.CreateLine(startX, startY, endX, endY)
      }
    },

    CreateLine(_startX, _startY, _endX, _endY) {
      const g = new PIXI.Graphics()
        .lineStyle(1, 0xffff00)
        .moveTo(_startX, _startY)
        .lineTo(_endX, _endY)

      this.myContainer.lines.addChild(g)
    },

    MyAnim() {
      this.myContainer.lines.removeChildren()
      this.DrawLine(0)
    }
  }
}
</script>

<style scoped lang="scss">
.c-line {
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
</style>
