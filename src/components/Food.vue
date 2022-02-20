<template>
  <div v-show="xCoord && yCoord">
    <q-icon
      ref="food"
      name="las la-pizza-slice"
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
      yCoord: 0
    }
  },
  mounted () {
    setTimeout(() => {
      this.generateCoordinate()
    }, 300)
  },
  computed: {
    stylePosition () {
      return `left: ${this.xCoord}px; top: ${this.yCoord}px; z-index: 99`
    }
  },
  methods: {
    generateCoordinate () {
      this.xCoord = this.randomIntFromInterval(5, this.screenWidth - 47)
      this.yCoord = this.randomIntFromInterval(5, this.screenHeight - 85)

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