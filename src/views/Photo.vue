<template>
  <v-container>
    <v-img :src="my_img"></v-img>
  </v-container>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Photo",
  data: function () {
    return {
      my_img: "lol",
    };
  },
  components: {},
  created() {
    console.log("test", this.$route.params.img);
    this.my_img = this.$route.params.img;
  },
};
</script>
