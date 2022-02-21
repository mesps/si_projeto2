<template>
  <div v-show="xCoord && yCoord">
    <q-icon
      ref="food"
      :name="`las la-${foodIcon}`"
      color="orange-9"
      size="32px"
      :style="stylePosition"
    />
  </div>
</template>

<script>
export default {
  name: 'Food',
  props: ['screenWidth', 'screenHeight'],
  data () {
    return {
      xCoord: 0,
      yCoord: 0,
      foodOptions:  ['pizza-slice', 'hamburger', 'cheese', 'cookie', 'lemon', 'carrot', 'hotdog', 'apple-alt'],
      foodIcon: ''
    }
  },
  mounted () {
    // Gera uma comida aleatória
    this.foodIcon = this.generateRandomFood()

    setTimeout(() => {
      // Gera uma posição aleatória
      this.generateCoordinate()
    }, 300)
  },
  computed: {
    stylePosition () {
      return `left: ${this.xCoord}px; top: ${this.yCoord}px; z-index: 99`
    }
  },
  methods: {
    generateRandomFood () {
      const index = this.randomIntFromInterval(0, 7)
      return this.foodOptions[index]
    },

    generateCoordinate () {
      this.xCoord = this.randomIntFromInterval(2, this.screenWidth - 49)
      this.yCoord = this.randomIntFromInterval(1, this.screenHeight - 100)

      this.emitCoordinates()
    },

    emitCoordinates () {
      setTimeout(() => {
        const x = this.$refs.food.$el.getBoundingClientRect().x
        const y = this.$refs.food.$el.getBoundingClientRect().y

        this.$emit('foodCoordinates', { x, y })
      }, 100)
    },

    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    }
  }
}
</script>

<style>

</style>