<template>
  <div>
    <h4 style="color: var(--black) !important;">{{ title }}</h4>
    <div class="mb-3">{{ description }}</div>
    <a-upload 
      list-type="picture"
      :fileList="files" 
      :multiple="multiple"
      :before-upload="beforeUpload"
      @remove="handleRemove"
      :capture="null"
    >
      <a-button class="mt-2">
        <upload-outlined />
        Subir Archivos
      </a-button>
    </a-upload>
  </div>
</template>

<script setup>
import { UploadOutlined } from '@ant-design/icons-vue'
import { ref } from 'vue'

const title = 'Documentos requeridos'
const description = 'Subí fotos, PDFs o archivos relevantes'
const multiple = true

const files = ref([
  {
    uid: '1',
    name: 'archivo1.pdf',
    status: 'done',
    url: 'https://example.com/archivo1.pdf',
  },
  {
    uid: '2',
    name: 'imagen.jpg',
    status: 'done',
    url: 'https://example.com/imagen.jpg',
  },
])

const handleRemove = file => {
  files.value = files.value.filter(item => item.uid !== file.uid)
}

const beforeUpload = file => {
  files.value = [...files.value, file]
  return false
}
</script>
