<template>
  <div class="flex items-center justify-center">
    <div v-if="uiType === '1'" class="card-container">
      <div class="card-DragNDrop">
        <div
          v-if="!previewFile && !formData.file"
          class="card-preview drag-area"
          @dragover.prevent="onDragOver"
          @dragleave.prevent="onDragLeave"
          @drop.prevent="onDrop"
        >
          <div>
            <div class="icon">
              <UIcon name="i-heroicons-cloud-arrow-up-16-solid" />
            </div>
            <div>
              <span
                v-if="!isDragging"
                class="select"
                role="button"
                @click="selectFiles"
                >กดเพื่อเพิ่ม</span
              >
              <span v-else>ปล่อยไฟล์ตรงนี้</span>
              <input
                type="file"
                name="file"
                :accept="typeFile"
                @change="onSelectFile"
                ref="fileInput"
                class="input"
              />
            </div>
          </div>
        </div>
        <div class="flex preview-container" v-else>
          <img
            class="preview"
            :src="previewFile"
            :style="{ objectFit: fitType }"
          />
          <UButton
            :ui="{ rounded: 'rounded-full' }"
            class="delete"
            @click="deleteFile"
            :padded="false"
            color="gray"
            variant="soft"
            icon="i-heroicons-x-mark-20-solid"
          />
        </div>
      </div>
    </div>
    <div
      v-if="uiType === '2'"
      class="card-container2 cursor-pointer opacity-100 hover:opacity-80"
    >
      <div class="card-DragNDrop2">
        <div
          class="card-preview2 drag-area2"
          @dragover.prevent="onDragOver2"
          @dragleave.prevent="onDragLeave2"
          @drop.prevent="onDrop2"
          @click="selectFiles2"
        >
          <div class="text-[#b1b1b1] text-sm">
            <span class="text-[#607CAA] underline">Click to Upload</span> or
            drag and drop
          </div>
          <input
            type="file"
            name="file"
            @change="onSelectFile2"
            ref="fileInput"
            class="input"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    uiType: {
      type: String,
      default: '1',
    },
    fitType: {
      type: String,
      default: 'cover',
    },
    limitSize: {
      type: Number,
      default: 300,
    },
    typeFile: {
      type: String,
      default: 'image/*',
    },
  },
  data() {
    return {
      formData: {
        file: '',
      },
      isDragging: false,
      previewFile: '',
      fileName: '',
    }
  },

  methods: {
    selectFiles() {
      this.$refs.fileInput.click()
    },
    onSelectFile(e) {
      const file = e.target.files[0]
      const fileInput = e.target

      const allowedTypes = this.typeFile?.split(',').map((type) => type.trim())
      console.log('Allowed Types:', allowedTypes)
      console.log('File Type:', file.type)

      const maxFileSize = parseInt(this.limitSize) * 1024 * 1024

      if (file.size > maxFileSize) {
        alert(`ไฟล์ควรมีขนาดไม่เกิน ${this.limitSize}MB`)
        fileInput.value = null
        return
      }

      const isValidFileType = allowedTypes.some((type) => {
        if (type === 'image/*') {
          return file.type.startsWith('image/')
        }
        return file.type === type
      })

      if (!isValidFileType) {
        const allowedTypeDescriptions = allowedTypes
          .map((type) => {
            if (type === 'image/*') return 'รูปภาพ (JPEG, PNG, JPG)'
            if (type === 'application/pdf') return 'ไฟล์ PDF'
            return type
          })
          .join(', ')

        alert(
          `ประเภทไฟล์ไม่ถูกต้อง\nประเภทไฟล์ที่รองรับ: ${allowedTypeDescriptions}`
        )
        return
      }

      this.formData.file = file

      if (file.type === 'application/pdf') {
        this.previewFile =
          'https://logowik.com/content/uploads/images/adobe-pdf3324.jpg'
      } else if (file.type.startsWith('image/')) {
        this.previewFile = URL.createObjectURL(file)
      }

      this.emitTestEvent()
    },

    onDragOver(e) {
      e.preventDefault()
      this.isDragging = true
      e.dataTransfer.dropEffect = 'copy'
    },

    onDragLeave(e) {
      e.preventDefault()
      this.isDragging = false
    },

    onDrop(e) {
      e.preventDefault()
      this.isDragging = false

      const files = e.dataTransfer.files
      if (!files || files.length === 0) {
        alert('กรุณาเลือกไฟล์')
        return
      }

      const file = files[0]
      const allowedTypes = this.typeFile?.split(',').map((type) => type.trim())
      console.log('Allowed Types:', allowedTypes)
      console.log('File Type:', file.type)

      const maxFileSize = parseInt(this.limitSize) * 1024 * 1024 // 300MB

      if (file.size > maxFileSize) {
        alert('ไฟล์ควรมีขนาดไม่เกิน 300MB')
        return
      }

      const isValidFileType = allowedTypes.some((type) => {
        if (type === 'image/*') {
          return file.type.startsWith('image/')
        }
        return file.type === type
      })

      if (!isValidFileType) {
        const allowedTypeDescriptions = allowedTypes
          .map((type) => {
            if (type === 'image/*') return 'รูปภาพ (JPEG, PNG, JPG)'
            if (type === 'application/pdf') return 'ไฟล์ PDF'
            return type // Fallback for other MIME types
          })
          .join(', ')

        alert(
          `ประเภทไฟล์ไม่ถูกต้อง\nประเภทไฟล์ที่รองรับ: ${allowedTypeDescriptions}`
        )
        return
      }

      this.formData.file = file

      if (file.type === 'application/pdf') {
        this.previewFile =
          'https://logowik.com/content/uploads/images/adobe-pdf3324.jpg'
      } else if (file.type.startsWith('image/')) {
        this.previewFile = URL.createObjectURL(file)
      }

      this.emitTestEvent()
    },

    deleteFile() {
      this.formData.file = ''
      this.previewFile = ''
      this.$emit('test', this.formData.file, this.previewFile)
    },
    emitTestEvent() {
      this.$emit('test', this.formData.file, this.previewFile)
    },

    selectFiles2() {
      this.$refs.fileInput.click()
    },
    onSelectFile2(e) {
      const file = e.target.files[0]

      if (!file) {
        alert('กรุณาเลือกไฟล์ที่ถูกต้อง')
        return
      }

      const allowedTypes = this.typeFile?.split(',').map((type) => type.trim())
      console.log('Allowed Types:', allowedTypes)
      console.log('File Type:', file.type)

      const maxFileSize = parseInt(this.limitSize) * 1024 * 1024 // 300MB

      // Check file size
      if (file.size > maxFileSize) {
        alert('ไฟล์ควรมีขนาดไม่เกิน 300MB')
        return
      }

      // Check file type with support for wildcards
      const isValidFileType = allowedTypes.some((type) => {
        if (type === 'image/*') {
          return file.type.startsWith('image/')
        }
        return file.type === type
      })

      if (!isValidFileType) {
        const allowedTypeDescriptions = allowedTypes
          .map((type) => {
            if (type === 'image/*') return 'รูปภาพ (JPEG, PNG, JPG)'
            if (type === 'application/pdf') return 'ไฟล์ PDF'
            return type // Fallback for other MIME types
          })
          .join(', ')

        alert(
          `ประเภทไฟล์ไม่ถูกต้อง\nประเภทไฟล์ที่รองรับ: ${allowedTypeDescriptions}`
        )
        return
      }

      this.fileName = file.name
      this.formData.file = file
      this.previewFile = URL.createObjectURL(file)

      // Reset the file input for subsequent uploads
      e.target.value = ''

      this.emitTestEvent2()
    },

    onDragOver2(e) {
      e.preventDefault()
      this.isDragging = true
      e.dataTransfer.dropEffect = 'copy'
    },

    onDragLeave2(e) {
  e.preventDefault();
  this.isDragging = false;
},

onDrop2(e) {
  e.preventDefault();
  this.isDragging = false;

  const file = e.dataTransfer.files[0];

  // Check if the file exists
  if (!file) {
    alert('กรุณาเลือกไฟล์ที่ถูกต้อง');
    return;
  }

  const allowedTypes = this.typeFile?.split(',').map((type) => type.trim());
  console.log('Allowed Types:', allowedTypes);
  console.log('File Type:', file.type);

  const maxFileSize = parseInt(this.limitSize) * 1024 * 1024; // 300MB

  if (file.size > maxFileSize) {
    alert('ไฟล์ควรมีขนาดไม่เกิน 300MB');
    return;
  }

  const isValidFileType = allowedTypes.some((type) => {
    if (type === 'image/*') {
      return file.type.startsWith('image/');
    }
    return file.type === type;
  });

  if (!isValidFileType) {
    const allowedTypeDescriptions = allowedTypes
      .map((type) => {
        if (type === 'image/*') return 'รูปภาพ (JPEG, PNG, JPG)';
        if (type === 'application/pdf') return 'ไฟล์ PDF';
        return type;
      })
      .join(', ');

    alert(`ประเภทไฟล์ไม่ถูกต้อง\nประเภทไฟล์ที่รองรับ: ${allowedTypeDescriptions}`);
    return;
  }

  this.formData.file = file;

  // Set preview for specific file types
  if (file.type === 'application/pdf') {
    this.previewFile = 'https://logowik.com/content/uploads/images/adobe-pdf3324.jpg';
  } else if (file.type.startsWith('image/')) {
    this.previewFile = URL.createObjectURL(file);
  }

  this.emitTestEvent2();
},


    deleteFile2() {
      this.formData.file = ''
      this.previewFile = ''
    },

    emitTestEvent2() {
      this.$emit('test', this.formData.file, this.previewFile, this.fileName)
    },
  },
}
</script>

<style scoped>
.card-container {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #ededed;
  width: 331px;
  height: 207px;
  border-radius: 4px;
}

.card {
  background-color: #f4f4f4;
  width: 1, 049px;
  height: 624px;
}

.card-DragNDrop {
  width: 900px;
  height: 240px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card-preview {
  height: 200px;
  width: 855.13px;
  top: 20px;
  left: 22.44px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.drag-area {
  width: 142.5px;
  height: 120px;
  border-radius: 4px;
  border: 2px dashed #b1b1b1;
}
.card-container2 {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #ededed;
  width: 100%;
  height: 100%;
  border-radius: 4px;
  border: 2px dashed #b1b1b1;
}
.card-DragNDrop2 {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.card-preview2 {
  width: 100%;
  height: 100%;
  min-height: 160px;
  top: 20px;
  left: 22.44px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.drag-area2 {
  width: 100%;
  height: 100%;
  border-radius: 4px;
}
.input {
  display: none;
}

.preview {
  width: 316px;
  height: 194px;
  border-radius: 12px;
  object-position: center;
  display: block;
}
.preview2 {
  width: 100%;
  max-height: 100%;
  min-height: 160px;
  margin: 16px 32px;
  border-radius: 12px;
  object-fit: cover;
  object-position: center;
  display: block;
  border-radius: 24px;
}

.delete {
  z-index: 999;
  color: white;
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  cursor: pointer;
  background: transparent;
}

.preview-container {
  width: 153px;
  position: relative;
  display: inline-block;
}
.preview-container2 {
  width: max-content;
  position: relative;
  display: inline-block;
}
.icon {
  color: #848484;
  font-size: 41px;
  display: flex;
  justify-content: center;
}
</style>
