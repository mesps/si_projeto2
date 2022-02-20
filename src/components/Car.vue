<template>
  <div v-show="xCoord && yCoord">
    <div
      ref="car"
      class="car"
      :style="stylePosition"
    />
  </div>
</template>

<script>
export default {
  name: 'Car',
  props: ['screenWidth', 'screenHeight', 'foodCoordinates'],
  data () {
    return {
      xCoord: 0,
      yCoord: 0,
      stepSize: 1,
      moveMilliseconds: 15,
      startMoviment: false
    }
  },
  computed: {
    stylePosition () {
      return `position: relative; left: ${this.xCoord}px; top: ${this.yCoord}px; z-index: 100`
    }
  },
  watch: {
    foodCoordinates: {
      deep: true,
      handler (val) {
        console.log(val)
        if (val) this.startMoviment = true
        else this.startMoviment = false
      }
    },
    startMoviment (val) {
      if (val) {
        setTimeout(() => {
          this.controlYMoviment()
        }, 200)
      }
    }
  },
  mounted () {
    setTimeout(() => {
      this.positionOnCenter()
    }, 100)

  },
  methods: {
    carPositionOnScreen () {
      const x = this.$refs.car.getBoundingClientRect().x
      const y = this.$refs.car.getBoundingClientRect().y

      return { x, y }
    },

    positionOnCenter () {
      this.xCoord = (this.screenWidth - 42) / 2
      this.yCoord = (this.screenHeight - 132) / 2
    },

    controlYMoviment () {
      const carCoordinates = this.carPositionOnScreen()
  
      if (carCoordinates.y < this.foodCoordinates.y) {
        this.moveY(this.moveDown)
      } else if (carCoordinates.y > this.foodCoordinates.y) {
        this.moveY(this.moveUp)
      }
    },

    moveY (moveFunction) {
      const interval = setInterval(() => {
        moveFunction()
  
        const carCoordinates = this.carPositionOnScreen()
        if (carCoordinates.y === this.foodCoordinates.y) {
          clearInterval(interval)
  
          this.controlXMoviment()
        }
      }, this.moveMilliseconds)
    },

    controlXMoviment () {
      const carCoordinates = this.carPositionOnScreen()

      if (carCoordinates.x < this.foodCoordinates.x) {
        this.moveX(this.moveRight)
      } else if (carCoordinates.x > this.foodCoordinates.x) {
        this.moveX(this.moveLeft)
      }
    },

    moveX (moveFunction) {
      const interval = setInterval(() => {
        moveFunction()
  
        const carCoordinates = this.carPositionOnScreen()
        if (carCoordinates.x === this.foodCoordinates.x) {
          clearInterval(interval)

          this.$emit('ateFood')
        }
      }, this.moveMilliseconds)
    },

    moveUp () {
      this.yCoord -= this.stepSize
    },

    moveDown () {
      this.yCoord += this.stepSize
    },

    moveRight () {
      this.xCoord += this.stepSize
    },

    moveLeft () {
      this.xCoord -= this.stepSize
    }
  }
}
</script>

<style lang="sass">
  .car
    width: 0
    height: 0
    border-left: 10px solid transparent
    border-right: 10px solid transparent
    border-bottom: 20px solid #003366
  
  .diretionRight
    transform: rotate(90deg)

  .diretionLeft
    transform: rotate(-90deg)
  
  .diretionDown
    transform: rotate(180deg)

  @keyframes rotateLeft
    100%
      transform: rotate(-90deg)
  
  @keyframes rotateRight
    100%
      transform: rotate(90deg)
  
  @keyframes rotateDown
    100%
      transform: rotate(180deg)

</style>