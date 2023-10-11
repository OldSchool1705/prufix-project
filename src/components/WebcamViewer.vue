<template>
  <div class="container mx-auto">
    <video ref="videoElement" autoplay></video>
    <div class="camera-status mb-10" :class="{ active: isCameraActive }">
      {{ isCameraActive ? 'Камера включена' : 'Камера выключена' }}
    </div>
    <button
      type="button"
      @click="startStreaming"
      class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
    >
      Начать трансляцию
    </button>
    <button
      type="button"
      @click="stopStreaming"
      class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
    >
      Остановить трансляцию
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      videoStream: null,
      isCameraActive: false
    };
  },
  methods: {
    async startStreaming() {
      try {
        this.videoStream = await navigator.mediaDevices.getUserMedia({ video: true });
        console.log(this.videoStream);
        this.$refs.videoElement.srcObject = this.videoStream;
        this.isCameraActive = true; // Показываем, что камера включена
      } catch (error) {
        console.error('Не удалось получить доступ к веб-камере: ', error);
      }
    },
    stopStreaming() {
      if (this.videoStream) {
        this.videoStream.getTracks().forEach((track) => track.stop());
        this.$refs.videoElement.srcObject = null;
        this.isCameraActive = false; // Показываем, что камера выключена
      }
    }
  }
};
</script>

<style>
.camera-status {
  margin-top: 10px;
  font-weight: bold;
}

.active {
  color: green;
}
</style>
