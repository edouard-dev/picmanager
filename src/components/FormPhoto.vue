<template>
  <v-container fluid class="pa-0 container-cam">

  </v-container>
</template>

<script>
export default {
  props: {
    image: {
      required: true,
    },
  },
  data: function () {
    return {
      stream: null,
      width: 0,
      height: 0,
      photo: true,
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
    mounted() {
      this.image.drawImage(
        document.querySelector("video"),
        0,
        0,
        picture.width,
        picture.height
      );
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
