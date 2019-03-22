<template>
  <div class="photoMainContainer" v-on:click="$emit('close')">
    <div class="loading">Loading...</div>
    <img class="photo" v-on:click.stop v-bind:src="getPhotoUrl()">
    <div class="close">Close</div>
  </div>
</template>

<script>
export default {
  name: 'Photo',
  props: [
    'item'
  ],
  methods: {
    getPhotoUrl: function () {
      const original = this.item.media.m
      // eslint-disable-next-line
      console.log("Photo url : " + this.item.media.m.substring(0, original.length - 5))
      return this.item.media.m.substring(0, original.length - 5) + 'b.jpg'
    }
  }
}
</script>

<style scoped>
.photoMainContainer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.photo {
  max-width: 80vw;
  max-height: 80vh;
  z-index: 10;
  cursor: default;
}
.loading {
  position: fixed;
  color: white;
  font-size: 18px;
}
.close {
  position: fixed;
  color: white;
  bottom: 20px;
  opacity: 0;
  font-weight: 900;
  transition: opacity 0.2s ease-in-out;
}
.photoMainContainer:hover .close {
  opacity: 1;
}
.photo:hover + .close {
  opacity: 0;
}
</style>