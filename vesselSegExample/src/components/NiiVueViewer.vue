<template>
  <div class="canvas-container">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<style scoped>
.canvas-container {
  width: 60vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>

<script>
import { onMounted, ref } from 'vue'
import { Niivue } from '@niivue/niivue'

export default {
  name: 'NiiVueViewer',
  props: {
    imageUrl: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const canvas = ref(null)

    onMounted(() => {
      const nv = new Niivue()
      nv.attachToCanvas(canvas.value)

      const volumeList = [
        {
          url: props.imageUrl,
          volume: { hdr: null, img: null },
          colorMap: 'gray',
          opacity: 1,
          visible: true,
        }
      ]

      nv.loadVolumes(volumeList)
    })

    return { canvas }
  }
}
</script>