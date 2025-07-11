<template>
  <div>
    <h1>xd</h1>
    <a-upload
      v-model:file-list="fileList"
      action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
      :before-upload="beforeUpload"
      @change="handleChange"
      :multiple="true"
      :show-upload-list="true"
      :accept="acceptedTypes"
    >
      <a-button>
        <upload-outlined />
        Subir archivos (imágenes, PDF, DOC, etc.)
      </a-button>
    </a-upload>
    
    <!-- input "manual" para máxima compatibilidad (opcional extra) -->
    <input
      ref="rawInput"
      type="file"
      :accept="acceptedTypes"
      multiple
      style="display: none"
      @change="handleManualFile"
    />
    <a-button class="mt-2" @click="triggerRawInput">
      📁 Elegir archivo (máxima compatibilidad)
    </a-button>
  </div>
</template>

<script>
import { UploadOutlined } from '@ant-design/icons-vue';
import { defineComponent, ref } from 'vue';
import { message, Upload } from 'ant-design-vue';

export default defineComponent({
  components: {
    UploadOutlined,
  },
  setup() {
    const fileList = ref([]);
    const rawInput = ref(null);

    const acceptedTypes = [
      'image/png',
      'image/jpeg',
      'image/jpg',
      'application/pdf',
      'application/msword',
      'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
      'text/plain',
    ].join(',');

    const handleChange = ({ file, fileList: newList }) => {
       console.log('Archivo actual:', file);
      console.log('Archivos cargados:', newList);
      fileList.value = newList;
    };

    const beforeUpload = (file) => {
      const isAccepted = acceptedTypes.includes(file.type);
      if (!isAccepted) {
        message.error(`${file.name} no es un tipo de archivo permitido.`);
      }
      return isAccepted || Upload.LIST_IGNORE;
    };

    const triggerRawInput = () => {
      rawInput.value?.click();
    };

    const handleManualFile = (event) => {
      const files = Array.from(event.target.files);
      files.forEach((file, index) => {
        console.log('Archivo seleccionado manualmente:', file);
        // Podrías subir manualmente aquí o hacer lo que necesites con ellos
        fileList.value.push({
          uid: `${Date.now()}-${index}`,
          name: file.name,
          status: 'done',
          url: URL.createObjectURL(file),
        });
      });
    };

    return {
      fileList,
      handleChange,
      beforeUpload,
      acceptedTypes,
      rawInput,
      triggerRawInput,
      handleManualFile,
    };
  },
});
</script>
