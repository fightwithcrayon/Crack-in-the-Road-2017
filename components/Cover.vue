<template>
  <section id="cover">
    <canvas id="sitetitle" ref="sitetitle" v-on:click="$router.push('/')"></canvas>
    <portal-target name="cover" slim>
    </portal-target>
  </section>
</template>
<script>
export default {
  props: ['image', 'title', 'caption', 'fontsReady'],
  computed: {
    cap () {
      return this.$props.caption //|| this.default.title.rendered
    },
    srcset () {
      return this.$props.image //|| this.default.featured_image_srcset
    },
    ttl () {
      return this.$props.title //|| this.default.title.rendered
    }
  },
  methods: {
    scribbleTitle(){
      var ctx = this.$refs.sitetitle.getContext("2d"),
        dashLen = 50, dashOffset = dashLen, speed = 10,
        txt = "CITR", x = 30, i = 0;

      ctx.font = "92px scriptorama-tradeshow-jf, sans-serif"; 
      ctx.lineWidth = 1; ctx.lineJoin = "round"; ctx.globalAlpha = 1;
      ctx.strokeStyle = ctx.fillStyle = "white";

      (function loop() {
        ctx.setLineDash([dashLen - dashOffset, dashOffset - speed]); // create a long dash mask
        dashOffset -= speed;                                         // reduce dash length
        ctx.strokeText(txt[i], x, 90);                               // stroke letter

        if (dashOffset > 0) requestAnimationFrame(loop);             // animate
        else {
          ctx.fillText(txt[i], x, 90);                               // fill final letter
          dashOffset = dashLen;                                      // prep next char
          x += ctx.measureText(txt[i++]).width + ctx.lineWidth * Math.random();
          ctx.setTransform(1, 0, 0, 1, 0, 3 * Math.random());        // random y-delta
          ctx.rotate(Math.random() * 0.005);                         // random rotation
          if (i < txt.length) requestAnimationFrame(loop);
        }
      })()
    }
  },
  watch: {
    'fontsReady': function (newQuestion, oldQuestion) {
      this.scribbleTitle()
    }
  }
}
</script>
<style lang="scss">
#cover {
  background-color: rgb(7,162,218);  
}
</style>