<template>
  <a-upload
    v-model:file-list="fileList"
    action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
    :before-upload="beforeUpload"
    @change="handleChange"
  >
    <a-button>
      <upload-outlined></upload-outlined>
      Upload png only
    </a-button>
  </a-upload>
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
    const fileList = ref([{
      uid: '1',
      name: 'xxx.png',
      status: 'done',
      response: 'Server Error 500',
      // custom error message to show
      url: 'http://www.baidu.com/xxx.png',
    }, {
      uid: '2',
      name: 'yyy.png',
      status: 'done',
      url: 'http://www.baidu.com/yyy.png',
    }, {
      uid: '3',
      name: 'zzz.png',
      status: 'error',
      response: 'Server Error 500',
      // custom error message to show
      url: 'http://www.baidu.com/zzz.png',
    }]);
    const handleChange = ({
      file,
      fileList,
    }) => {
      if (file.status !== 'uploading') {
        console.log(file, fileList);
      }
    };
    const beforeUpload = file => {
      const isPNG = file.type === 'image/png';
      if (!isPNG) {
        message.error(`${file.name} is not a png file`);
      }
      return isPNG || Upload.LIST_IGNORE;
    };
    return {
      fileList,
      handleChange,
      beforeUpload,
    };
  },
});
</script>