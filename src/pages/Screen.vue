<template>
  <q-page class="flex flex-center">
    <q-card flat bordered class="screen q-pa-md" ref="screen">
      <span class="text-black text-bold text-h5" ref="foodCounter">
        Comida: {{ foodCounter }}
      </span>
      <!-- {{ boundingClientRect }} -->
      <food
        :rightLimit="rightLimit"
        :leftLimit="leftLimit"
        :topLimit="topLimit"
        :bottomLimit="bottomLimit"
        :width="width"
        :height="height"
      />
    </q-card>
    <q-resize-observer @resize="onResize" />
  </q-page>
</template>

<script>
import food from 'components/Food'

export default {
  name: 'PageIndex',
  components: {
    food
  },
  data () {
    return {
      foodCounter: 0,
      boundingClientRect: null
    }
  },
  computed: {
    rightLimit () {
      return (this.boundingClientRect && this.boundingClientRect.right) || 0
    },
    leftLimit () {
      return (this.boundingClientRect && this.boundingClientRect.left) || 0
    },
    topLimit () {
      return (this.boundingClientRect && this.boundingClientRect.top) || 0
    },
    bottomLimit () {
      return (this.boundingClientRect && this.boundingClientRect.bottom) || 0
    },
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
  }
}
</script>

<style lang="sass">
 .screen
  height: 400px
  width: 600px
  border: 2px solid #00023C
  background-color: #DEDEDE

</style>
