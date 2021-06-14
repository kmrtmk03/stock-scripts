<template lang="pug">
  .c-test
    .canvasWrap(ref='canvas')
</template>


<script>

let PIXI
if(process.client) {
  PIXI = require('pixi.js')
}

export default {

  data() {
    return {
      scrollAmo: 0,
      app: null,
      el: null,
      windowSize: [
        {width: 0},
        {height: 0}
      ],
      myContainer: [
        { objects: null },
        { scroll: null },
        { guide: null }
      ],
      obj1: null,
      myScroll: [
        { base: null },
        { bar: null }
      ]
    }
  },

  mounted() {

    this.ScrollNotArrow()

    this.InitParm()

    this.InitPixi()

    window.addEventListener('wheel', (e) => {
      this.ActionScroll(e)
    })
  },

  methods: {

    InitParm() {
      this.windowSize.width = document.body.clientWidth
      this.windowSize.height = document.body.clientHeight
    },

    ActionScroll(e) {
      this.scrollAmo += e.deltaY / 1

      const myMax = 10000
      const myMin = 0

      if(this.scrollAmo <= myMin) {
        this.scrollAmo = myMin
      }
      if(myMax <= this.scrollAmo) {
        this.scrollAmo = myMax
      }

      const cpm = this.scrollAmo / myMax
      const pageWidth = 4000 - this.windowSize.width

      this.myContainer.objects.x = pageWidth * cpm * -1
      this.myContainer.guide.x = pageWidth * cpm * -1

      this.myScroll.bar.x = cpm * (this.windowSize.width - 20)

      console.log('current position is \n [ ' + this.myContainer.objects.x * -1 + ' ]')

    },

    InitPixi() {
      this.app = new PIXI.Application({
        width: this.windowSize.width,
        height: this.windowSize.height,
        backgroundColor: 0xeeeeee
      })

      this.el = this.$refs.canvas
      this.el.appendChild(this.app.view)

      this.InitContainer()

      this.CreateScrollPosition()

      for(let i = 0; i < 10; i++) {
        this.CreateEllipse(30, 500 * i + 500, 0xff0000, i)
      }

      for(let i = 0; i < 10; i++) {
        this.CreateGuideline(-500 * i)
      }
    },

    InitContainer() {

      this.myContainer.guide = new PIXI.Container()
      this.myContainer.guide.x = 0
      this.myContainer.guide.y = 0
      this.myContainer.guide.pivot.x = 0
      this.app.stage.addChild(this.myContainer.guide)

      this.myContainer.objects = new PIXI.Container()
      this.myContainer.objects.x = 0
      this.myContainer.objects.y = this.windowSize.height - 110
      this.myContainer.objects.pivot.x = 0
      this.app.stage.addChild(this.myContainer.objects)

      this.myContainer.scroll = new PIXI.Container()
      this.myContainer.scroll.x = 0
      this.myContainer.scroll.y = this.windowSize.height - 100
      this.myContainer.scroll.pivot.x = 0
      this.app.stage.addChild(this.myContainer.scroll)
    },

    CreateEllipse(_size, _posX, _color, _index) {
      this.obj1 = new PIXI.Graphics()
        .beginFill(_color)
        .drawEllipse(0, 0, _size, _size)
        .endFill()

      this.obj1.pivot.x = 0
      this.obj1.pivot.y = _size

      this.obj1.x = _posX
      this.obj1.y = 0

      this.myContainer.objects.addChild(this.obj1)

      this.obj1.interactive = true

      this.obj1.on('click', () => {
        this.ActionClick(_index)
      })
    },

    CreateScrollPosition() {

      const scrollHeight = 100

      this.myScroll.base = new PIXI.Graphics()
        .beginFill(0x666666)
        .drawRect(0, 0, this.windowSize.width, scrollHeight)
        .endFill()

      this.myScroll.base.pivot.x  = 0
      this.myScroll.base.pivot.y  = 0

      this.myScroll.base.x = 0
      this.myScroll.base.y = 0

      this.myContainer.scroll.addChild(this.myScroll.base)

      this.myScroll.bar = new PIXI.Graphics()
        .beginFill(0xffffff)
        .drawRect(0, 0, 20, 90)
        .endFill()

      this.myScroll.bar.pivot.x = 0
      this.myScroll.bar.pivot.y = 0

      this.myScroll.bar.y = 5;

      this.myContainer.scroll.addChild(this.myScroll.bar)
    },

    CreateGuideline(_posX) {
      const _myGuide = new PIXI.Graphics()
        .beginFill(0xff0000)
        .drawRect(0, 0, 1, this.windowSize.height)
        .endFill()

      _myGuide.x = 0
      _myGuide.y = 0

      _myGuide.pivot.x = _posX
      _myGuide.pivot.y = 0

      this.myContainer.guide.addChild(_myGuide)
    },

    ActionClick(_index) {
      console.log("My index is " + _index)
    },

    NoScroll(e) {
      e.preventDefault()
    },

    ScrollNotArrow() {
      document.addEventListener('touchmove', this.NoScroll, { passive: false });
      document.addEventListener('mousewheel', this.NoScroll, { passive: false });
    }
  }
}
</script>

<style scoped lang="scss">
  .c-test {
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }
</style>
