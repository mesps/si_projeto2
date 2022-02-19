<template>
  <q-page class="flex flex-center">
    <q-card flat bordered class="screen q-pa-md">
      <span class="text-black text-bold text-h5" ref="foodCounter">
        Comida: {{ foodCounter }}
      </span>
    </q-card>
  </q-page>
</template>

<script>

export default {
  name: 'PageIndex',
  data () {
    return {
      foodCounter: 0
    }
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
