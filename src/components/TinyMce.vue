<template>
  <Editor v-model="innerValue" :init="setUpTiny()" />
</template>

<script>
import 'tinymce/tinymce'
import 'tinymce/icons/default/icons'
import 'tinymce/themes/silver/theme'
import 'tinymce/models/dom/model'
import 'tinymce/skins/ui/oxide/skin.css'
import contentUiCss from 'tinymce/skins/ui/oxide/content.css';

import 'tinymce/plugins/insertdatetime/plugin'
import 'tinymce/plugins/anchor/plugin'
import 'tinymce/plugins/nonbreaking/plugin'
import 'tinymce/plugins/pagebreak/plugin'
import 'tinymce/plugins/charmap/plugin'
import 'tinymce/plugins/table/plugin'
import 'tinymce/plugins/codesample/plugin'
import 'tinymce/plugins/template/plugin'
import 'tinymce/plugins/media/plugin'
import 'tinymce/plugins/link/plugin'
import 'tinymce/plugins/image/plugin'
import 'tinymce/plugins/fullscreen/plugin'
import 'tinymce/plugins/visualchars/plugin'
import 'tinymce/plugins/visualblocks/plugin'
import 'tinymce/plugins/code/plugin'
import 'tinymce/plugins/directionality/plugin'
import 'tinymce/plugins/autolink/plugin'
import 'tinymce/plugins/searchreplace/plugin'
import 'tinymce/plugins/importcss/plugin'
import 'tinymce/plugins/preview/plugin'
import 'tinymce/plugins/advlist/plugin'
import 'tinymce/plugins/lists/plugin'
import 'tinymce/plugins/wordcount/plugin'
import 'tinymce/plugins/quickbars/plugin'
import 'tinymce/plugins/help/plugin'

import 'tinymce/plugins/emoticons/plugin'
import 'tinymce/plugins/emoticons/js/emojis'

import Editor from '@tinymce/tinymce-vue'

const specialChars = [
  { text: 'Nome do aluno', value: '@[aluno_nome]' },
  { text: 'Turma', value: '@[turma]' },
  { text: 'Nome do pai', value: '@[aluno_pai]' },
  { text: 'Nome da mãe', value: '@[aluno_mae]' },
  { text: 'Assinatura do aluno', value: '@[aluno_assinatura]' },
  { text: 'Assinatura do responsável 1', value: '@[responsavel_1_assinatura]' },
  { text: 'Assinatura do responsável 2', value: '@[responsavel_2_assinatura]' },
];

export default {
  components: {
    Editor,
  },
  data() {
    return {
      innerValue: '',
    };
  },
  props: {
    value: {
      type: String,
      default: '',
    },
  },
  methods: {
    setUpTiny() {
      return {
        skin: false,
        plugins: 'preview importcss searchreplace autolink directionality code visualblocks visualchars fullscreen image link media template codesample table charmap pagebreak nonbreaking anchor insertdatetime advlist lists wordcount help charmap quickbars emoticons',
        toolbar: `undo redo | bold italic underline strikethrough | fontfamily fontsize blocks | alignleft aligncenter alignright alignjustify | outdent indent |  numlist bullist | forecolor backcolor removeformat | pagebreak | charmap emoticons | fullscreen  preview save print | insertfile image media template link anchor codesample`,
        content_css: false,
        emoticons_database: 'emojis',
        content_style: contentUiCss.toString(),
        promotion: false,
        setup(editor) {
          const onAction = (autocompleteApi, rng, value) => {
            editor.selection.setRng(rng);
            editor.insertContent(value);
            autocompleteApi.hide();
          };

          const getMatchedChars = (pattern) => {
            pattern = pattern.toLowerCase();
            return specialChars.filter((char) => {
              return char.text.toLowerCase().indexOf(pattern.slice(1)) !== -1
                || char.value.toLowerCase().indexOf(pattern) !== -1;
            });
          };

          /* An autocompleter that allows you to insert special characters */
          editor.ui.registry.addAutocompleter('specialchars', {
            ch: '@',
            minChars: 1,
            columns: '2',
            onAction: onAction,
            fetch: (pattern) => {
              return new Promise((resolve) => {
                resolve(
                  getMatchedChars(pattern).map((char) => {
                    return {
                      type: 'cardmenuitem',
                      value: char.value,
                      label: char.text,
                      items: [
                              {
                                type: 'cardcontainer',
                                direction: 'vertical',
                                items: [
                                  {
                                    type: 'cardtext',
                                    text: char.text,
                                    name: 'char_name'
                                  },
                                  {
                                    type: 'cardtext',
                                    text: char.value
                                  }
                                ]
                              }
                            ],
                    }
                  })
                );
              });
            }
          });
        }
      };
    },
  },
  watch: {
    innerValue(value) {
      this.$emit('input', value);
    },
  },
};
</script>