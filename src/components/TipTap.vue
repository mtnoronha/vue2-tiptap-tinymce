<template>
  <div class="editor-container" v-if="editor">
    <div class="toolbar">
      <i
        class="fa fa-bold"
        @click="editor.chain().focus().toggleBold().run()"
        :class="{ 'is-active': editor.isActive('bold') }"
        :title="$t('label.bold')"
      />

      <i
        class="fa fa-italic"
        @click="editor.chain().focus().toggleItalic().run()"
        :class="{ 'is-active': editor.isActive('italic') }"
        :title="$t('label.italic')"
      />

      <i
        class="fa fa-strikethrough"
        @click="editor.chain().focus().toggleStrike().run()"
        :class="{ 'is-active': editor.isActive('strike') }"
        :title="$t('label.strikethrough')"
      />

      <i
        class="ri r-title"
        @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
        :title="$t('label.header.1')"
      >
      <span class="title-number">1</span>
      </i>

      <i
        class="ri r-title"
        @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }"
        :title="$t('label.header.2')"
      >
      <span class="title-number">2</span>
      </i>

      <i
        class="ri r-title"
        @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }"
        :title="$t('label.header.3')"
      >
      <span class="title-number">3</span>
      </i>

      <i
        class="fa fa-link"
        @click="setLink"
        :class="{ 'is-active': editor.isActive('link') }"
        :title="$t('label.link')"
      />

      <i
        class="fa fa-list"
        @click="editor.chain().focus().toggleBulletList().run()"
        :class="{ 'is-active': editor.isActive('bulletList') }"
        :title="$t('label.list.unordered')"
      />

      <i
        class="fa fa-list-ol"
        @click="editor.chain().focus().toggleOrderedList().run()"
        :class="{ 'is-active': editor.isActive('orderedList') }"
        :title="$t('label.list.ordered')"
      />

      <i
        class="fa fa-code"
        @click="editor.chain().focus().toggleCodeBlock().run()"
        :class="{ 'is-active': editor.isActive('codeBlock') }"
        :title="$t('label.code')"
      />

      <i
        class="fa fa-undo"
        @click="editor.chain().focus().undo().run()"
        :title="$t('label.undo')"
      />

      <i
        class="fa fa-redo"
        @click="editor.chain().focus().redo().run()"
        :title="$t('label.redo')"
      />
    </div>

    <editor-content :editor="editor" />
  </div>
</template>

<script>
import { Editor, EditorContent } from '@tiptap/vue-2';
import StarterKit from '@tiptap/starter-kit';
import Link from '@tiptap/extension-link';

export default {
  components: {
    EditorContent,
  },
  props: {
    value: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      editor: null,
    };
  },
  methods: {
    $t(label) {
      return label;
    },
    $sanitize(html) {
      return html;
    },
    setLink() {
      const url = window.prompt('URL');

      this.editor.chain().focus().setLink({ href: url }).run();
    },
  },
  watch: {
    value(value) {
      const isSame = this.editor.getHTML() === value;

      if (isSame || !this.editor.focused) {
        return;
      }

      this.editor.commands.setContent(this.$sanitize(this.value), false);
    },
  },
  mounted() {
    this.editor = new Editor({
      extensions: [
        StarterKit,
        Link,
      ],
      autofocus: 'end',
      content: this.value,
      onUpdate: () => {
        this.$emit('input', this.$sanitize(this.editor.getHTML()));
      },
      onBlur: () => {
        this.$emit('blur');
      },
    });
  },
  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>
<style scoped>
  .editor-container {
    position: relative;
    width: 100%;
  }

  .editor-container i {
    border: 1px solid transparent;
    border-radius: 10px;
    color: var(--color-dark-grey);
    cursor: pointer;
    padding: 10px 15px;
    transition: var(--color-transition);
  }

  .editor-container >>> .ProseMirror {
    min-height: 200px;
    padding: 5px;
  }

  .editor-container i:hover {
    box-shadow: var(--box-shadow-deep);
    font-size: 20px;
    margin: -10px 0;
  }

  .editor-container i.is-active {
    box-shadow: var(--box-shadow-deep);
    border: 1px solid var(--color-dark-grey);
  }

  .editor-container >>> .ProseMirror ul, .editor-container >>> .ProseMirror ol {
    padding-left: 20px;
  }

  .editor-container >>> .ProseMirror ul {
    list-style-type: disc;
  }

  .toolbar {
    background: white;
    border-radius: 5px;
    position: sticky;
    padding: 5px;
    margin-bottom: 20px;
    text-align: center;
    top: 2px;
    z-index: 1;
  }

  .title-number {
    font-size: 13px;
    font-weight: bold;
  }
</style>
