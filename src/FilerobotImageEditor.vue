<template>
  <div id="editor_container"/>
</template>
<script>
import FilerobotImageEditor from "./assets/lib/filerobot-image-editor.min.js";

export default {
  props: {
    src: {
      type: String,
      required: true,
    },
    config: {
      type: Object,
      default: () => ({}),
    },
  },
  mounted() {
    this.init();
  },
  methods: {
    init: function () {
      // config parameters
      const config = this.config;
      config.finishButtonLabel = this.config.finishButtonLabel || "save";
      config.source = this.src;

      // Registering this.callbacks
      this.callbacks = {};
      const vm = this;
      // @modify
      this.callbacks.onModify = (imageDesignState) => {
        vm.$emit("modify", imageDesignState);
      };
      // @error
      this.callbacks.onError = (error) => {
        vm.$emit("error", error);
      };
      // @close
      this.callbacks.onClose = (status) => {
        vm.$emit("close", status);
      };
      // @beforeSave
      this.callbacks.onBeforeSave = function (imageFileInfo) {
        vm.$emit("beforeSave", imageFileInfo);
        return false;
      };
      // @save
      this.callbacks.onSave = (editedImageObject, designState) => {
        this.$emit("save", editedImageObject, designState);
      };

      //render
      const container = document.getElementById("editor_container");
      this.imageEditor = new FilerobotImageEditor(container, config);
      this.imageEditor.render(this.callbacks);
    },
  },
  destroy() {
    if (this.imageEditor) {
      this.imageEditor.unmounted();
    }
  },
};
</script>
