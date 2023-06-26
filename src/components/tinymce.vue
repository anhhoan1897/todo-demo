<template>
    <editor v-model="editorValue" :init="init"></editor>
  </template>
  
  <script setup>
  import { reactive, ref, watch, toRefs } from 'vue';
  
  import tinymce from 'tinymce/tinymce.js';
  
  import 'tinymce/skins/ui/oxide/skin.css';
  import 'tinymce/themes/silver';
  
  import 'tinymce/icons/default';
  
  import 'tinymce/plugins/emoticons';
  import 'tinymce/plugins/emoticons/js/emojis.js';
  import 'tinymce/plugins/table';
  import 'tinymce/plugins/quickbars';
  
  
  // TinyMCE-Vue
  import Editor from '@tinymce/tinymce-vue';
  
  const props = defineProps({
    modelValue: {
      type: String,
      default: '',
    },
    plugins: {
      type: [String, Array],
      default: 'quickbars emoticons table',
    },
    toolbar: {
      type: [String, Array],
      default:
        ' bold italic underline strikethrough | fontsizeselect | forecolor backcolor | alignleft aligncenter alignright alignjustify|bullist numlist |outdent indent blockquote | undo redo | axupimgs | removeformat | table | emoticons',
    },
  });
  
  const emit = defineEmits(['update:modelValue']);
  
  const init = reactive({
    height: 300,
    menubar: false,
    content_css: false,
    skin: false,
    plugins: props.plugins,
    toolbar: props.toolbar,
    quickbars_insert_toolbar: false,
    branding: false,
  });
  
  const { modelValue } = toRefs(props);
  const editorValue = ref(modelValue.value);
  
  watch(modelValue, (newValue) => {
    editorValue.value = newValue;
  });
  
  watch(editorValue, (newValue) => {
    emit('update:modelValue', newValue);
  });
  </script>
  