<template>
  <v-container fluid class="pa-0 container-cam" v-if="!next">
    <video autoplay class="feed" v-show="photo"></video>
    <v-btn
      class="mx-2 btn-close"
      fab
      dark
      large
      color="grey"
      :style="cssVars"
      @click="closePhoto"
      v-if="!photo"
    >
      <v-icon dark> mdi-close </v-icon>
    </v-btn>
    <v-btn
      class="mx-2 btn-cam"
      fab
      dark
      large
      color="grey"
      :style="cssVars"
      @click="takePhoto"
      v-if="photo"
    >
      <v-icon dark> mdi-camera </v-icon>
    </v-btn>
    <v-btn
      class="mx-2 btn-next"
      fab
      dark
      large
      color="grey"
      :style="cssVars"
      @click="nextPhoto"
      v-if="!photo"
    >
      <v-icon dark> mdi-arrow-right </v-icon>
    </v-btn>
    <v-btn color="grey" fab dark class="btn-galery">
      <v-icon>mdi-menu</v-icon>
    </v-btn>
    <canvas
      :style="cssVars"
      class="picture"
      :width="width"
      :height="height"
      v-show="!photo"
    >
    </canvas>
  </v-container>
</template>

<script>
export default {
  props: {},
  components: {},
  data: function () {
    return {
      stream: null,
      width: 0,
      height: 0,
      photo: true,
      next: false,
      my_image: null,
    };
  },
  methods: {
    async init() {
      if (
        "mediaDevices" in navigator &&
        "getUserMedia" in navigator.mediaDevices
      ) {
        let constraints = {
          video: {
            height: { max: this.height },
            width: { max: this.width },
            aspectRatio: 0.6,
          },
        };
        const stream = await navigator.mediaDevices
          .getUserMedia(constraints)
          .then((stream) => {
            const video = document.querySelector("video");
            video.srcObject = stream;
            video.play();
          });
      }
    },
    getDevices: async function () {
      const devices = await navigator.mediaDevices.enumerateDevices();
    },
    handleResize() {
      this.width = window.innerWidth;
      this.height = window.innerHeight;
    },
    closePhoto() {
      this.photo = true;
    },
    nextPhoto() {
      this.$router.push({ name: "Photo", params: { img: this.my_image } });
    },
    takePhoto() {
      this.photo = false;
      const picture = document.querySelector("canvas");
      const ctx = picture.getContext("2d");
      ctx.imageSmoothingEnabled = true;
      ctx.imageSmoothingQuality = "high";
      ctx.drawImage(
        document.querySelector("video"),
        0,
        0,
        picture.width,
        picture.height
      );
      this.my_image = picture.toDataURL("image/jpeg", 1.0);
      console.log("je suis imporant", this.my_image);
    },
  },
  beforeMount() {
    this.init();
  },
  created() {
    window.addEventListener("resize", this.handleResize);
    this.handleResize();
  },

  computed: {
    cssVars() {
      return {
        "--left-cam": this.width / 2 - 28 + "px",
        "--height": this.height + "px",
        "--width": this.width + "px",
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.feed {
  width: 100%;
  display: block;
}

.container-cam {
  height: 100%;
  width: 100%;
  background: black;
  position: fixed;
}

.btn-cam {
  position: fixed;
  bottom: 20px;
  left: var(--left-cam);
}

.btn-close {
  position: fixed;
  top: 20px;
  left: 20px;
}

.btn-galery {
  position: fixed;
  top: 20px;
  right: 20px;
}

.btn-next {
  position: fixed;
  bottom: 20px;
  right: 20px;
}
.picture {
  width: var(--width);
  height: var(--height);
}
</style>
