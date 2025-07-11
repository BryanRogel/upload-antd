<template>
  <div>
    <a-upload
      v-model:file-list="fileList"
      :custom-request="customUpload"
      :before-upload="beforeUpload"
      @change="handleChange"
      :multiple="true"
      :accept="acceptTypes"
    >
      <a-button>
        <upload-outlined />
        Subir archivo (fotos, PDF, DOC...)
      </a-button>
    </a-upload>
  </div>
</template>

<script>
import { UploadOutlined } from '@ant-design/icons-vue'
import { defineComponent, ref } from 'vue'
import { message } from 'ant-design-vue'

export default defineComponent({
  components: {
    UploadOutlined,
  },
  setup() {
    const fileList = ref([])

    // Acepta imágenes, PDF, Word, texto, etc.
    const acceptTypes = [
      'image/*',
      'application/pdf',
      'application/msword',
      'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
      'text/plain'
    ].join(',')

    // 🔧 Personalizar subida para evitar problemas móviles
    const customUpload = ({ file, onSuccess }) => {
      // Simular subida con un timeout
      setTimeout(() => {
        const fakeUrl = URL.createObjectURL(file)
        fileList.value.push({
          uid: `${Date.now()}`,
          name: file.name,
          status: 'done',
          url: fakeUrl,
        })
        onSuccess("ok")
      }, 500)
    }

    const beforeUpload = (file) => {
      const isValid = acceptTypes.includes(file.type) || file.type.startsWith("image/")
      if (!isValid) {
        message.error(`${file.name} no es un archivo válido`)
      }
      return isValid
    }

    const handleChange = ({ fileList: newList }) => {
      fileList.value = newList
    }

    return {
      fileList,
      acceptTypes,
      beforeUpload,
      handleChange,
      customUpload,
    }
  },
})
</script>
