<template>
  <div v-show="xCoord && yCoord">
    <div
      ref="car"
      class="car"
      :style="stylePosition"
      :class="rotationClass + ' ' + directionClass"
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
      moveMilliseconds: 12,
      rotateMilliseconds: 200,
      startMoviment: false,
      rotationClass: 'rotate0',
      directionClass: 'directionUp'
    }
  },
  computed: {
    stylePosition () {
      return `position: relative; left: ${this.xCoord}px; top: ${this.yCoord}px; z-index: 100`
    },

    direction () {
      return {
        down: this.directionClass === 'directionDown',
        up: this.directionClass === 'directionUp',
        right: this.directionClass === 'directionRight',
        left: this.directionClass === 'directionLeft'
      }
    }
  },
  watch: {
    foodCoordinates: {
      deep: true,
      handler (val) {
        // Começa o movimento quando recebe as coordenadas da comida
        if (val) this.startMoviment = true
        else this.startMoviment = false
      }
    },
    startMoviment () {
      if (this.startMoviment) {
        setTimeout(() => {
          // Começa se movimentando no eixo Y
          this.controlYMoviment()
        }, 200)
      }
    }
  },
  mounted () {
    setTimeout(() => {
      // Posiciona o carro no centro
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

    rotateAndMove (rotationClass, directionClass, moveFunction) {
      // Adiciona classe CSS que gira o carro
      this.rotationClass = rotationClass

      setTimeout(() => {
        this.rotationClass = ''

        // Adiciona classe CSS que corresponde a direção em que o carro se encontras
        this.directionClass = directionClass

        moveFunction()
      }, this.rotateMilliseconds)
    },

    controlYMoviment () {
      const carCoordinates = this.carPositionOnScreen()
      const moveFunctionDown = () => this.moveY(this.moveDown)
      const moveFunctionUp = () => this.moveY(this.moveUp)

      // go down
      if (carCoordinates.y < this.foodCoordinates.y) {
        if (this.direction.down) this.moveY(this.moveDown)
        else if (this.direction.right) this.rotateAndMove('rotate90', 'directionDown', moveFunctionDown)
        else if (this.direction.left) this.rotateAndMove('rotateMinus90', 'directionDown', moveFunctionDown)
        else if (this.direction.up) this.rotateAndMove('rotate180', 'directionDown', moveFunctionDown)

      // go up
      } else if (carCoordinates.y > this.foodCoordinates.y) {
        if (this.direction.up) this.moveY(this.moveUp)
        else if (this.direction.left) this.rotateAndMove('rotate90', 'directionUp', moveFunctionUp)
        else if (this.direction.right) this.rotateAndMove('rotateMinus90', 'directionUp', moveFunctionUp)
        else if (this.direction.down) this.rotateAndMove('rotate180', 'directionUp', moveFunctionUp)
      } 
      else this.controlXMoviment()
    },

    moveY (moveFunction) {
      // Um loop de intervalos que para apenas quando chega na posição certa de Y
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
      const moveFunctionLeft = () => this.moveX(this.moveLeft)
      const moveFunctionRight = () => this.moveX(this.moveRight)

      // go to right
      if (carCoordinates.x < this.foodCoordinates.x) {
        if (this.direction.right) this.moveX(this.moveRight)
        else if (this.direction.up) this.rotateAndMove('rotate90', 'directionRight', moveFunctionRight)
        else if (this.direction.down) this.rotateAndMove('rotateMinus90', 'directionRight', moveFunctionRight)
        else if (this.direction.left) this.rotateAndMove('rotate180', 'directionRight', moveFunctionRight)

      // go to left
      } else if (carCoordinates.x > this.foodCoordinates.x) {
        if (this.direction.left) this.moveX(this.moveLeft)
        else if (this.direction.down) this.rotateAndMove('rotate90', 'directionLeft', moveFunctionLeft)
        else if (this.direction.up) this.rotateAndMove('rotateMinus90', 'directionLeft', moveFunctionLeft)
        else if (this.direction.right) this.rotateAndMove('rotate180', 'directionLeft', moveFunctionLeft)
      }
    },

    moveX (moveFunction) {
      // Um loop de intervalos que para apenas quando chega na posição certa de X
      const interval = setInterval(() => {
        moveFunction()
  
        const carCoordinates = this.carPositionOnScreen()
        if (carCoordinates.x === this.foodCoordinates.x) {
          clearInterval(interval)

          // Emite evento de colisão com a comida
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

  .directionUp
    border-left: 10px solid transparent
    border-right: 10px solid transparent
    border-bottom: 20px solid #003366
  
  .directionDown
    border-left: 10px solid transparent
    border-right: 10px solid transparent
    border-top: 20px solid #003366

  .directionLeft
    border-top: 10px solid transparent
    border-bottom: 10px solid transparent 
    border-right: 20px solid #003366

  .directionRight
    border-top: 10px solid transparent
    border-bottom: 10px solid transparent
    border-left: 20px solid #003366

  .rotate90
    animation: rotate90 0.2s
    animation-fill-mode: forwards

  .rotate180
    animation: rotate180 0.2s
    animation-fill-mode: forwards

  .rotateMinus90
    animation: rotateMinus90 0.2s
    animation-fill-mode: forwards

  @keyframes rotate90
    100%
      transform: rotate(90deg)
  
  @keyframes rotate180
    100%
      transform: rotate(180deg)

  @keyframes rotateMinus90
    100%
      transform: rotate(-90deg)
</style>