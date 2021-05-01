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
    canvasDraw() {
      const canvasWidth = 400
      const canvasHeight = 400
      // canvas内の要素をクリア
      this.context.clearRect(0, 0, canvasWidth, canvasHeight)
      // canvas上に画像を表示
      const img = new Image()
      img.src = this.uploadedImage
      img.onload = () => {
        this.canvas.width = canvasWidth
        this.canvas.height = img.height * (canvasWidth / img.width)
        this.context.drawImage(
          img,
          0,
          0,
          canvasWidth,
          img.height * (canvasWidth / img.width)
        )
        this.addText(canvasWidth, img)
        this.createLink()
      }
    },
    // 画像上にテキストを表示
    addText(canvasWidth, img) {
      this.context.font = "900 60px 'MS Pゴシック'"
      this.context.textAlign = 'center'
      this.context.textBaseline = 'middle'
      this.context.fillStyle = '#FFFFFF'
      this.context.fillText(
        'LGTM',
        canvasWidth / 2,
        (img.height * (canvasWidth / img.width)) / 2
      )
    },
    // ダウンロードリンクを生成して出力
    createLink() {
      const parent = document.getElementById('result')
      // result内の要素をクリア
      if (parent.firstChild) {
        parent.removeChild(parent.firstChild)
      }
      const dlLink = document.createElement('a')
      dlLink.href = this.canvas.toDataURL()
      dlLink.download = 'LGTM.png'
      dlLink.textContent = 'ダウンロード'
      dlLink.className = 'download'
      parent.appendChild(dlLink)
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
