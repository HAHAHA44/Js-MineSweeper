<template>
  <div id="mine-swapper">
    <canvas id="ms-canvas" :width="boardSize" :height="boardSize"></canvas>
  </div>
</template>

<script>

const MINE_SIZE = 10;

export default {
  name: 'MineSwapper',
  props: ["difficulty"],
  data() {
    return {
      mineList: [],
    }
  },
  methods: {
    generateMines() {
      let array = new Uint16Array(this.mineQuantity * 2);
      window.crypto.getRandomValues(array);
      array = Array.from(array.map(i => Math.floor(i % this.boardSize / 10) * 10));
      this.mineList = array.filter((num, index) => index % 2 === 0).map((num, index) => [num, array[index + 1]]);
    },
    deduplicationMines() {
      let dedupGraph = new Array(this.boardSize);
      for (let i = 0; i < this.boardSize; i++) {
        dedupGraph[i] = new Array(this.boardSize);
      }
      const resetMines = (cur, circuleNum) => {
        let currentInd = 0;
        if (circuleNum >= this.mineQuantity) {
          currentInd = 1;
        }
        if (!dedupGraph[cur[0]][cur[1]]) {
          dedupGraph[cur[0]][cur[1]] = true;
          return cur;
        } else {
          cur[currentInd] = (cur[currentInd] + 10) % this.boardSize;
          return resetMines(cur, circuleNum + 1);
        }
      }
      for (let i = 0; i < this.mineList.length; i++) {
        this.mineList[i] = resetMines(this.mineList[i], 1);
      }
    },
    drawGrid(ctx) {
      for (let i = 1; i < this.mineQuantity; i++) {
        ctx.beginPath();
        ctx.moveTo(i * 10, 0);
        ctx.lineTo(i * 10, this.boardSize);
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(0, i * 10);
        ctx.lineTo(this.boardSize, i * 10);
        ctx.stroke();
      }
    }
  },
  mounted() {
    const canvas = document.getElementById('ms-canvas');
    const ctx = canvas.getContext('2d');
    this.generateMines();
    this.deduplicationMines();
    this.drawGrid(ctx);
    ctx.fillStyle = "rgb(200,0,0)";
    for (let i = 0;i < this.mineList.length; i++) {
      ctx.fillRect(this.mineList[i][0], this.mineList[i][1], MINE_SIZE, MINE_SIZE);
    }
  },
  computed: {
    boardSize() {
      if (this.difficulty === 'easy') {
        return 100;
      } else if (this.difficulty === 'common') {
        return 300;
      } else return 500;
    },
    mineQuantity() {
      return this.boardSize / 10;
    }
  }
}
</script>

<style>
  canvas {
    border: 1px solid black;
    width: 500px;
    height: 500px;
  }
</style>