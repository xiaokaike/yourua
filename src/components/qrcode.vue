<template>
  <div>
    <canvas
      :style="{height: size + 'px', width: size + 'px'}"
      height={{size}}
      width={{size}}
      v-el:qr
    ></canvas>
  </div>
</template>


<script>
import qr from 'qr.js'

export default {
  props: {
    val: String,
    size: 10,
    // 'L', 'M', 'Q', 'H'
    level: String,
    bgc: String,
    fgc: String
  },
  ready () {
    var size = this.size
    var bgc = this.bgc
    var fgc = this.fgc
    var $qr = this.$els.qr

    var qrcode = qr(this.val)

    var ctx = $qr.getContext('2d')
    var cells = qrcode.modules
    var tileW = size / cells.length
    var tileH = size / cells.length
    var scale = (window.devicePixelRatio || 1) / getBackingStorePixelRatio(ctx)

    $qr.height = $qr.width = size * scale
    ctx.scale(scale, scale)

    cells.forEach(function (row, rdx) {
      row.forEach(function (cell, cdx) {
        ctx.fillStyle = cell ? fgc : bgc
        var w = (Math.ceil((cdx + 1) * tileW) - Math.floor(cdx * tileW))
        var h = (Math.ceil((rdx + 1) * tileH) - Math.floor(rdx * tileH))
        ctx.fillRect(Math.round(cdx * tileW), Math.round(rdx * tileH), w, h)
      })
    })
  }
}

function getBackingStorePixelRatio (ctx) {
  return (
    ctx.webkitBackingStorePixelRatio ||
    ctx.mozBackingStorePixelRatio ||
    ctx.msBackingStorePixelRatio ||
    ctx.oBackingStorePixelRatio ||
    ctx.backingStorePixelRatio ||
    1
  )
}

</script>

<style>
  

</style>