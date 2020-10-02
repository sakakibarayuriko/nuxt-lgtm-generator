<template>
  <div id="app" class="container">
    <div>
      <h1>LGTM Generator</h1>
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
      uploadedImage: '',
    }
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
      canvas.width = canvasWidth
      canvas.height = canvasHeight
      const ctx = canvas.getContext('2d')
      // canvas内の要素をクリア
      ctx.clearRect(0, 0, canvasWidth, canvasHeight)

      // canvas上に画像を表示
      const img = new Image()
      img.src = this.uploadedImage
      img.onload = function () {
        ctx.drawImage(
          img,
          0,
          0,
          canvasWidth,
          img.height * (canvasWidth / img.width)
        )

        // canvas上にテキストを表示
        ctx.font = "bold 30px 'MS Pゴシック'"
        ctx.textAlign = 'center'
        ctx.textBaseline = 'middle'
        ctx.fillStyle = '#FFFFFF'
        ctx.fillText('L G T M', canvasWidth / 2, canvasHeight / 2)

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
        dlLink.download = 'sample.png'
        dlLink.textContent = 'ダウンロード'
        parent.appendChild(dlLink)
      }
    },
  },
}
</script>

<style>
h1 {
  margin: 30px;
  color: #35495e;
  font-size: 30px;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  /* align-items: center; */
  text-align: center;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
