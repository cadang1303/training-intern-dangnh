<template>
  <div class="main">
    <div
      class="dropzone-container"
      :class="{ 'drop-active': isDragging }"
      @dragover="dragover"
      @dragleave="dragleave"
      @drop="drop"
    >
      <input
        type="file"
        name
        id="fileInput"
        class="hidden-input"
        ref="file"
        @change="onChange"
        multiple
        accept=".pdf, .jpg, .jpeg, .png"
      />
      <label for="fileInput" class="file-label">
        <img class="upload-icon" src="@/assets/icon/interfaces/upload.png" />
        <div v-if="isDragging" class="drag-text">
          Release to drop files here.
        </div>
        <div v-else>
          <div class="drag-text">Drag and drop files</div>
          <div class="click-input-text">Browse files</div>
        </div>
      </label>
    </div>
    <div class="preview-container" v-if="files.length">
      <div v-for="file in files" :key="file.name" class="preview-card">
        <img class="preview-icon" :src="generateURL(file)" />
        <div class="file-content">
          <div class="file-name">{{ file.name }}</div>
          <div class="file-size">{{ Math.round(file.size / 1000) + "kb" }}</div>
        </div>
        <div @click="remove(file)" class="file-cancel">
            <img src="@/assets/icon/interfaces/close-circle.png" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDragging: false,
      files: [],
    };
  },
  methods: {
    onChange() {
      this.files = [...this.$refs.file.files];
    },
    dragover(e) {
      e.preventDefault();
      this.isDragging = true;
    },
    dragleave() {
      this.isDragging = false;
    },
    drop(e) {
      e.preventDefault();
      this.$refs.file.files = e.dataTransfer.files;
      this.onChange();
      this.isDragging = false;
    },
    remove(i) {
      this.files.splice(i, 1);
    },
    generateURL(file) {
      let fileSrc = URL.createObjectURL(file);
      setTimeout(() => {
        URL.revokeObjectURL(fileSrc);
      }, 3000);
      return fileSrc;
    },
  },
};
</script>

<style scoped>
.main {
  font-family: "Noto Sans";
  font-style: normal;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.dropzone-container {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  width: 842px;
  height: 232px;
  padding: 4rem;
  background: #f8f8f8;
  border: 1px solid #dcdcdc;
  border-radius: 7px;
}
/* .drop-active {
  background: #336699;
  color: #fff;
} */
.drag-text {
  font-weight: 700;
  font-size: 18px;
  line-height: 22px;
  color: #333333;
}
.click-input-text {
  font-weight: 400;
  font-size: 18px;
  line-height: 22px;
  color: #333333;
  text-decoration: underline;
}
.upload-icon {
  margin-bottom: 20px;
}
.hidden-input {
  opacity: 0;
  overflow: hidden;
  position: absolute;
  width: 1px;
  height: 1px;
}
.file-label {
  font-size: 20px;
  display: block;
  cursor: pointer;
}
.preview-container {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  margin-top: 2rem;
}
.preview-card {
  display: flex;
  background: #ffffff;
  border: 1px solid #dcdcdc;
  border-radius: 3px;
  width: 244px;
  height: 48px;
  padding: 5px;
  margin-left: 5px;
}
.preview-icon {
  flex-grow: 1;
  padding: 8px;
  width: 24px;
  height: 32px;
}
.preview-content {
  display: flex;
  padding: 8px;
  flex-grow: 2;
  flex-direction: column;
  justify-content: left;
}
.file-name {
  font-size: 12px;
  font-weight: 700;
  line-height: 16px;
  color: #333333;
  width: 125px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.file-size {
  font-size: 10px;
  font-weight: 400;
  line-height: 14px;
  color: #666666;
}
.file-cancel {
  flex-grow: 1;
  width: 12.67px;
  height: 12.67px;
  margin: auto;
  margin-left: 30px;
  align-items: right;
}
</style>