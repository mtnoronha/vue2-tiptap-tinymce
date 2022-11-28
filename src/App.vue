<template>
  <div id="app">
    Teste do TipTap:
    <TipTap v-model="text"/>

    <hr>
    {{ text }}
    <hr>
    <editor :init="setUpTiny()" />
  </div>
</template>

<script>
// TinyMCE
import 'tinymce/tinymce'
import 'tinymce/icons/default/icons'
import 'tinymce/themes/silver/theme'
import 'tinymce/models/dom/model'
import 'tinymce/skins/ui/oxide/skin.css'
import contentUiCss from 'tinymce/skins/ui/oxide/content.css';

// TinyMCE plugins
// https://www.tiny.cloud/docs/tinymce/6/plugins/
import 'tinymce/plugins/lists/plugin'
import 'tinymce/plugins/link/plugin'
import 'tinymce/plugins/image/plugin'
import 'tinymce/plugins/table/plugin'
import 'tinymce/plugins/code/plugin'
import 'tinymce/plugins/help/plugin'
import 'tinymce/plugins/wordcount/plugin'

import Editor from '@tinymce/tinymce-vue'

import TipTap from './components/TipTap.vue';

export default {
  name: 'App',
  data() {
    return {
      text: 'Empty for now...',
    };
  },
  components: {
    Editor, TipTap
  },
  methods: {
    setUpTiny() {
      return {
        skin: false,
        plugins: 'lists link image table code help wordcount',
        content_css: false,
        content_style: contentUiCss.toString(),
      };
    },
  },
}
</script>
<style scoped>
  hr {
    margin-top: 50px;
    margin-bottom: 20px;
  }
</style>
