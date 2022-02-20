<template>
  <q-page class="flex flex-center">
    <q-card flat bordered class="screen q-pa-md" ref="screen">
      <span class="text-black text-bold text-h5 counter" ref="foodCounter">
        Comida: {{ foodCounter }}
      </span>
      <car
        :screenWidth="width"
        :screenHeight="height"
        :foodCoordinates="foodCoordinates"
        @ateFood="nextFood"
      />
      <food
        v-if="showFood"
        :screenWidth="width"
        :screenHeight="height"
        @foodCoordinates="coordinates => foodCoordinates = coordinates"
      />
    </q-card>
    <q-resize-observer @resize="onResize" />
  </q-page>
</template>

<script>
import food from 'components/Food'
import car from 'components/Car'

export default {
  name: 'PageIndex',
  components: {
    food,
    car
  },
  data () {
    return {
      foodCounter: 0,
      foodCoordinates: null,
      showFood: true,
      boundingClientRect: null
    }
  },
  computed: {
    width () {
      return (this.boundingClientRect && this.boundingClientRect.width) || 0
    },
    height () {
      return (this.boundingClientRect && this.boundingClientRect.height) || 0
    }
  },
  mounted () {
    this.boundingClientRect = this.$refs.screen.$el.getBoundingClientRect()
  },
  methods: {
    onResize (size) {
      this.boundingClientRect = this.$refs.screen.$el.getBoundingClientRect()
    },

    nextFood () {
      this.foodCounter += 1
      this.showFood = false
      this.foodCoordinates = null

      setInterval(() => {
        this.showFood = true
      }, 100)
    }
  }
}
</script>

<style lang="sass">
 .screen
  height: 400px
  width: 600px
  min-width: 600px
  border: 2px solid #00023C
  background-color: #DEDEDE

  .counter
  position: static

</style>
