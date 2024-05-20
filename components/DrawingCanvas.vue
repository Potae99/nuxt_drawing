<template>
    <v-container fluid>
      <v-card>
        <v-card-title>Drawing App</v-card-title>
        <v-card-text>
          <v-btn @click="clearCanvas">Clear</v-btn>
          <v-divider></v-divider>
          <div class="canvas-container">
            <canvas 
              ref="canvas" 
              @mousedown="startDrawing" 
              @mouseup="stopDrawing" 
              @mousemove="draw" 
              @touchstart="startDrawing" 
              @touchend="stopDrawing" 
              @touchmove="draw">
            </canvas>
          </div>
        </v-card-text>
      </v-card>
    </v-container>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isDrawing: false,
        ctx: null
      }
    },
    methods: {
      getCoordinates(event) {
        if (event.touches) {
          const touch = event.touches[0];
          return {
            x: touch.clientX - this.$refs.canvas.getBoundingClientRect().left,
            y: touch.clientY - this.$refs.canvas.getBoundingClientRect().top
          };
        } else {
          return {
            x: event.clientX - this.$refs.canvas.getBoundingClientRect().left,
            y: event.clientY - this.$refs.canvas.getBoundingClientRect().top
          };
        }
      },
      startDrawing(event) {
        this.isDrawing = true;
        const coords = this.getCoordinates(event);
        this.ctx.beginPath();
        this.ctx.moveTo(coords.x, coords.y);
      },
      stopDrawing() {
        this.isDrawing = false;
        this.ctx.closePath();
      },
      draw(event) {
        if (!this.isDrawing) return;
        event.preventDefault(); // Prevent scrolling on touch devices
        const coords = this.getCoordinates(event);
        this.ctx.lineTo(coords.x, coords.y);
        this.ctx.stroke();
      },
      clearCanvas() {
        this.ctx.clearRect(0, 0, this.$refs.canvas.width, this.$refs.canvas.height);
      },
      resizeCanvas() {
        const canvas = this.$refs.canvas;
        canvas.width = canvas.clientWidth;
        canvas.height = canvas.clientHeight;
      }
    },
    mounted() {
      const canvas = this.$refs.canvas;
      this.ctx = canvas.getContext('2d');
      this.ctx.strokeStyle = '#000';
      this.ctx.lineWidth = 2;
      this.resizeCanvas();
      window.addEventListener('resize', this.resizeCanvas);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.resizeCanvas);
    }
  }
  </script>
  
  <style>
  .canvas-container {
    width: 100%;
    height: 500px;
    position: relative;
    background: #FFFF;
  }
  canvas {
    width: 100%;
    height: 100%;
    border: 1px solid #000;
    cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M3 17.25V21h3.75l11.06-11.06-3.75-3.75L3 17.25M21.41 6.34a1 1 0 0 0 0-1.41l-2.34-2.34a1 1 0 0 0-1.41 0l-1.83 1.83 3.75 3.75 1.83-1.83z"/></svg>') 0 24, auto;
    display: block;
    touch-action: none; /* Prevent touch scrolling on canvas */
  }
  </style>
  