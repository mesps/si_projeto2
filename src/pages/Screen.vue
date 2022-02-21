<template>
  <q-page :class="$q.screen.gt.xs ? 'flex flex-center' : 'column q-pa-md'">
    <q-card flat bordered class="screen q-pa-md" :class="$q.screen.gt.xs ? 'rect' : 'col'" ref="screen">
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

    cleanFood () {
      this.showFood = false
      this.foodCoordinates = null
    },

    addToCounter () {
      this.foodCounter += 1
    },

    createNewFood () {
      setInterval(() => {
        this.showFood = true
      }, 100)
    },

    nextFood () {
      // Contabiliza
      this.addToCounter()

      // Limpa
      this.cleanFood()

      // Gera nova comida
      this.createNewFood()
    }
  }
}
</script>

<style lang="sass">
 .rect
  height: 400px
  width: 600px
  min-width: 600px
  
 .screen
  border: 2px solid #00023C
  background-color: #DEDEDE

  .counter
  position: static

</style>
