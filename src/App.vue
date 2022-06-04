<template>
  <div id="app">
    <img alt="Vue logo"
         src="./assets/logo.png"
         style="width:64px;height:64px"/>
    <img v-if="!openEditor" :src="url" @click="handleOpenEditor"/>
    <FilerobotImageEditor
      v-if="openEditor"
      :src="url"
      @modify="handleModify"
      @close="handleClose"
      @save="handleSave"
      @error="handleError"
      @beforeSave="onBeforeSave"
    />
  </div>
</template>
<script>
/* eslint-disable */
import FilerobotImageEditor from "./index";

export default {
  name: "App",
  components: {
    FilerobotImageEditor,
  },
  data() {
    return {
      url:
        "https://www.debian.org/Pics/debian-logo-1024x576.png",
      openEditor: false,
    };
  },
  mounted() {
  },
  methods: {
    handleModify(imgData) {
      console.info('imgData', imgData)
    },
    handleOpenEditor() {
      this.openEditor = true;
    },
    handleCloseEditor() {
      this.openEditor = false;
    },
    handleClose(status) {
      console.error('Close' + status)
      this.handleCloseEditor();
    },
    onBeforeSave(imageFileInfo) {
      console.error('imageFileInfo', imageFileInfo)
    },
    handleSave(editedImageObject, designState) {
      console.log("editedImageObject", editedImageObject)
      console.log("designState", designState)
      this.url = editedImageObject.imageCanvas.toDataURL();
      console.log('canvas data url', this.url)
      console.log('watermark', designState.annotations.watermark)
      //this.handleCloseEditor();
    },
    handleError(error) {
      this.handleCloseEditor();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
