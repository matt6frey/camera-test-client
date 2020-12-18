<template>
  <div class="hello">
    <div>
      <video ref="stream" class="stream" autoplay /><br />
      <canvas ref="display" class="stream display"></canvas><br />
      <button @click="takePhoto">TAKE PHOTO</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data: () => ({
    stream: null
  }),
  mounted() {
    const vm = this;
    navigator.mediaDevices
      .getUserMedia({ video: { facingMode: "user" } })
      .then(stream => {
        vm.$refs.stream.srcObject = stream;
        vm.stream = stream;
      })
      .catch(error => alert(error));
  },
  methods: {
    takePhoto(e) {
      e.preventDefault();
      const vm = this;

      const image = this.stream.getVideoTracks()[0];
      const imageCap = new ImageCapture(image);
      imageCap.takePhoto().then(blob => {
        console.log(blob);
        const canvas = vm.$refs.display;
        const ctx = canvas.getContext("2d");
        console.log(canvas.width, canvas.height);
        const img = new Image();
        img.onload = () => {
          ctx.drawImage(img, 0, 0, 320, 242);
        };
        img.src = window.URL.createObjectURL(blob);
        console.log(vm.$refs.stream.srcObject);
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.stream {
  max-width: 320px;
  height: auto;
  border: solid 1px black;
}

.display {
  height: 320px;
  width: 320px;
}
</style>
