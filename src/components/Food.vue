<template>
  <div v-show="xCoord && yCoord">
    <q-icon name="las la-pizza-slice" color="orange-9" size="32px" :style="stylePosition"/>
  </div>
</template>

<script>
export default {
  name: 'Food',
  props: ['topLimit', 'bottomLimit', 'rightLimit', 'leftLimit', 'width', 'height'],
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
      this.xCoord = this.randomIntFromInterval(5, this.width - 47)
      this.yCoord = this.randomIntFromInterval(5, this.height - 85)
    },
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min)
    }
  }
}
</script>

<style>

</style>