<template>
  <div id="app" class="container">
    <div>
      <div class="title">
        <h1 class="lgtm">LGTM</h1>
        <h1>Generator</h1>
      </div>
      <div>
        <input id="file" type="file" accept="image/*" @change="onFileChange" />
      </div>
      <canvas id="canvas"></canvas>
      <div id="result"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      canvas: null,
      context: null,
      uploadedImage: '',
    }
  },
  mounted() {
    this.canvas = document.getElementById('canvas')
    this.context = this.canvas.getContext('2d')
  },
  methods: {
    onFileChange(e) {
      const files = e.target.files || e.dataTransfer.files
      this.createImage(files[0])
    },
    // 画像を表示
    createImage(file) {
      const reader = new FileReader()
      // ファイル読み込みに成功したときの処理
      reader.onload = (e) => {
        this.uploadedImage = e.target.result
        this.canvasDraw()
      }
      reader.readAsDataURL(file)
    },
    // canvas上に画像を生成
    canvasDraw(imgSrc) {
      const canvas = document.getElementById('canvas')
      const canvasWidth = 400
      const canvasHeight = 400
      const ctx = canvas.getContext('2d')
      // canvas内の要素をクリア
      ctx.clearRect(0, 0, canvasWidth, canvasHeight)
      // canvas上に画像を表示
      const img = new Image()
      img.src = this.uploadedImage
      img.onload = function () {
        canvas.width = canvasWidth
        canvas.height = img.height * (canvasWidth / img.width)
        ctx.drawImage(
          img,
          0,
          0,
          canvasWidth,
          img.height * (canvasWidth / img.width)
        )

        // canvas上にテキストを表示
        ctx.font = "900 60px 'MS Pゴシック'"
        ctx.textAlign = 'center'
        ctx.textBaseline = 'middle'
        ctx.fillStyle = '#FFFFFF'
        ctx.fillText(
          'LGTM',
          canvasWidth / 2,
          (img.height * (canvasWidth / img.width)) / 2
        )

        // canvasを画像に変換
        const data = canvas.toDataURL()

        // ダウンロードリンクを生成して出力
        const parent = document.getElementById('result')
        // result内の要素をクリア
        if (parent.firstChild) {
          parent.removeChild(parent.firstChild)
        }
        const dlLink = document.createElement('a')
        dlLink.href = data
        dlLink.download = 'LGTM.png'
        dlLink.textContent = 'ダウンロード'
        dlLink.className = 'download'
        parent.appendChild(dlLink)
      }
    },
  },
}
</script>

<style>
h1 {
  color: #35495e;
  font-size: 40px;
  font-weight: normal;
  display: inline;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  text-align: center;
}

#canvas {
  margin: 20px;
}

.download {
  background-color: #526488;
  color: #ffffff;
  border-radius: 5px;
  padding: 10px;
  text-decoration: none;
  cursor: pointer;
}

.lgtm {
  font-weight: 800;
}

.title {
  margin-top: 30px;
  margin-bottom: 40px;
}
</style>
