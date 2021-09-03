<template>
  <div class="container">
    <h3>Click the image below!</h3>

    <div class="img_wrap">
      <p v-show="!checkBool">Loading...</p>
      <img
        v-show="checkBool"
        @click="on_image_click"
        ref="click_image"
        src=""
        alt=""
      />
    </div>
  </div>
</template>

<script>
import screenfull from "screenfull"
export default {
  data() {
    return {
      //   imgURL:
      //     "https://worksheets.mathsbuilder.com.au/worksheets/Language+cards/3/3_01/z/1024/whiteboard.jpg",
      imageObj: undefined,
      checkBool: false,
    }
  },
  mounted() {
    this.loadImage()
  },
  watch: {
    checkBool(newValue, oldValue) {
      if (newValue) {
        let isFinished
        this.imageObj.complete && this.imageObj.naturalWidth !== 0
          ? (isFinished = true)
          : (isFinished = false)

        if (isFinished) {
          this.$refs.click_image.src = this.imageObj.src

          console.log("Set the src!")
        }
      }
    },
  },
  methods: {
    loadImage() {
      ;(async () => {
        // console.log("loading: " + image + " url " + imageUrl)
        const img = new Image()
        img.setAttribute("crossOrigin", "anonymous")
        img.src = "https://source.unsplash.com/random/1600x900"
        await img.decode()

        // img is ready to use
        console.log(img + " has been loaded")
        console.log(
          img + " x: " + img.naturalWidth + ", y:" + img.naturalHeight
        )
        this.imageObj = img
        this.checkBool = true
        console.log(this.checkBool)
      })()
    },
    on_image_click() {
      if (screenfull.isEnabled) {
        // console.log("enabled!")
        // screenfull.toggle()
        screenfull
          .toggle(this.$refs.click_image)
          .then(function () {
            console.log("Should be full screen")
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-width: 100vw;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;

  .img_wrap {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 70vh;
    img {
      width: 70vw;
      /* height: 80vh; */

      cursor: pointer;
    }
  }
}
</style>
