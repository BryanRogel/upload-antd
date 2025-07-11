<template>
  <div>
    <h1>xdxdxd</h1>
    <a-button @click="triggerNativeInput">
      <upload-outlined /> Subir archivo
    </a-button>

    <!-- input nativo oculto que sí funciona bien en móviles -->
    <input
      ref="realInput"
      type="file"
      :accept="accept"
      multiple
      style="display: none"
      @change="handleNativeUpload"
    />

    <ul>
      <li v-for="file in fileList" :key="file.name">{{ file.name }}</li>
    </ul>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue'
import { UploadOutlined } from '@ant-design/icons-vue'

export default defineComponent({
  components: { UploadOutlined },
  setup() {
    const fileList = ref([])
    const realInput = ref(null)
    const accept = [
      'image/*',
      'application/pdf',
      'application/msword',
      'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
      'text/plain'
    ].join(',')

    const triggerNativeInput = () => {
      realInput.value?.click()
    }

    const handleNativeUpload = (e) => {
      const files = Array.from(e.target.files)
      fileList.value = files
      console.log('Archivos seleccionados:', files)
    }

    return {
      fileList,
      accept,
      realInput,
      triggerNativeInput,
      handleNativeUpload,
    }
  }
})
</script>
