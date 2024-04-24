<script setup>
import { onMounted, ref } from 'vue'

const edge = 560
const outputEdgeSize = 28
const cellColor = 'rgb(0 0 0)'
const gridColor = 'rgb(224 224 230)'
const surroundingCellsColor = 'rgb(0 0 0 / 0.05)'
const gridSize = edge / outputEdgeSize

const canvasEl = ref()
let isDrawing = ref(false)
let context = null

onMounted(() => {
  context = canvasEl.value.getContext('2d')
  drawGrid()
})

function drawGrid() {
  context.beginPath()
  context.strokeStyle = gridColor
  const numberOfCells = edge / gridSize
  for (let i = 0; i < numberOfCells; i++) {
    for (let j = 0; j < numberOfCells; j++) {
      context.strokeRect(i * gridSize, j * gridSize, gridSize, gridSize)
    }
  }
  context.stroke()
}

function clearCanvas() {
  context.clearRect(0, 0, edge, edge)
  drawGrid()
}

function check() {
  // convert the canvas to an image 28x28 pixels
  // send the image to the backend
}

function drawCell(e) {
  isDrawing.value = true
  draw(e)
}

function draw(e) {
  if (!isDrawing.value) return

  const rect = canvasEl.value.getBoundingClientRect()
  // Calculate the top-left corner of the grid cell to draw in
  const gridX = Math.floor((e.clientX - rect.left) / gridSize) * gridSize
  const gridY = Math.floor((e.clientY - rect.top) / gridSize) * gridSize

  // Fill the cell
  context.fillStyle = cellColor
  context.fillRect(gridX, gridY, gridSize, gridSize)

  fillSurroundingCells(gridX, gridY)
}

function fillSurroundingCells(x, y) {
  context.fillStyle = surroundingCellsColor
  // Loop through surrounding cells
  for (let i = -1; i <= 1; i++) {
    for (let j = -1; j <= 1; j++) {
      // Avoid overwriting the center cell
      if (i === 0 && j === 0) continue
      context.fillRect(x + i * gridSize, y + j * gridSize, gridSize, gridSize)
    }
  }
}
</script>

<script>
import { defineComponent } from 'vue'
import { NButton, NFlex } from 'naive-ui'

export default defineComponent({
  components: {
    NButton,
    NFlex
  }
})
</script>

<template>
  <n-flex vertical>
    <canvas
      ref="canvasEl"
      v-bind="{ width: edge, height: edge }"
      @mousemove="draw"
      @mouseout="isDrawing = false"
      @mouseup="isDrawing = false"
      @mousedown="drawCell"
    />
    <n-button @click="clearCanvas" size="large">Clear</n-button>
    <n-button @click="check" size="large" type="success">Check</n-button>
  </n-flex>
</template>

<style scoped>
canvas {
  border: 1px solid #36ad6a;
}
</style>
