<template>
  <v-app>
    <v-main>
      <DrawingCanvas />
    </v-main>
  </v-app>
</template>

<script>
import DrawingCanvas from '~/components/DrawingCanvas.vue'

export default {
  components: {
    DrawingCanvas
  }
}
</script>
