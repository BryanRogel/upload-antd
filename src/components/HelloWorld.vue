<template>
  <div>
    <template v-if="props.title">
      <h4 style="color: var(--black) !important;">{{ props.title }}</h4>
    </template>
    <slot v-else name="title" />
    <template v-if="props.description">
      <div class="mb-3">{{ props.description }}</div>
    </template>
    <slot v-else name="description" />

    <a-upload
      list-type="picture"
      :file-list="files"
      :multiple="props.multiple"
      :before-upload="() => false"
      @remove="handleRemove"
    >
      <a-button class="mt-2" @click.prevent="triggerInput">
        <upload-outlined />
        Subir Archivos
      </a-button>
    </a-upload>

    <input
      ref="fileInput"
      type="file"
      :multiple="props.multiple"
      :accept="accept"
      style="display: none"
      @change="handleManualUpload"
    />
  </div>
</template>

<script setup>
import { UploadOutlined } from '@ant-design/icons-vue'
import { defineProps, defineEmits, ref, watch, onUnmounted } from 'vue'

const props = defineProps({
  files: {
    type: Array,
    required: true,
    default: () => []
  },
  title: {
    type: String,
    default: null
  },
  description: {
    type: String,
    default: null
  },
  multiple: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['update:files'])
const files = ref([])
const fileInput = ref(null)

const accept = [
  'image/*',
  'application/pdf',
  'application/msword',
  'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  'text/plain'
].join(',')

const triggerInput = () => {
  fileInput.value?.click()
}

const handleManualUpload = (e) => {
  const selected = Array.from(e.target.files).map((file, index) => ({
    uid: `${Date.now()}-${index}`,
    name: file.name,
    status: 'done',
    url: URL.createObjectURL(file)
  }))
  files.value = props.multiple ? [...files.value, ...selected] : [selected[0]]
  emit('update:files', files.value)
}

const handleRemove = file => {
  files.value = files.value.filter(f => f.uid !== file.uid)
  emit('update:files', files.value)
}

watch(() => props.files, (val) => {
  files.value = val
}, { immediate: true })

onUnmounted(() => {
  files.value = []
})
</script>
