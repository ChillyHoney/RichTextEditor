<template>
  <div>
    <bubble-menu
      class="bubble-menu"
      :tippy-options="{ duration: 100 }"
      :editor="editor"
      v-if="editor"
    >
      <button @click="editor.chain().focus().toggleBold().run()" :class="{ 'is-active': editor.isActive('bold') }">
        Bold
      </button>
      <button @click="editor.chain().focus().toggleItalic().run()" :class="{ 'is-active': editor.isActive('italic') }">
        | Italic
      </button>
      <button @click="setLink" :class="{ 'is-active': editor.isActive('link') }">
        | Link
      </button>
      <button @click="editor.chain().focus().unsetLink().run()" v-if="editor.isActive('link')">
        | Remove
      </button>
    </bubble-menu>

    <editor-content :editor="editor" />
  </div>
</template>

<script>
import { Editor,  
EditorContent, 
BubbleMenu, 
 
} from '@tiptap/vue-3'
import Document from '@tiptap/extension-document'
import Paragraph from '@tiptap/extension-paragraph'
import Text from '@tiptap/extension-text'
import Link from '@tiptap/extension-link'
import Bold from '@tiptap/extension-bold'

export default {
  components: {
    EditorContent,
    BubbleMenu,
    
  },

  data() {
    return {
      editor: null,
    }
  },


 mounted() {
    this.editor = new Editor({
     extensions: [
        Document,
        Paragraph,
        Text,
        Bold,
        Link,
      ],
      content: `
        <p>
          Test it out! Select some of text and experience rich text editor.
        </p>
        <p>
          Vue.js rocks!
        </p>
      `,
    })
  },

   methods: {
    setLink() {
      const url = window.prompt('URL\n\nRemember about protocol: https://___', 'https://')

      this.editor
        .chain()
        .focus()
        .extendMarkRange('link')
        .setLink({ href: url })
        .run()
    },
  },

  beforeUnmount() {
    this.editor.destroy()
  },
}
</script>

<style lang="css">
/* Basic editor styles */
 
 .bubble-menu {
	display: flex;
	background-color: #f7f7f7;
	padding: 0.4rem;
    border-width: 0.4rem;
    border-color: #295bff;    
    border-style: outset;
	
}
 .bubble-menu button {
	border: 1px;
	background: none;
	color: rgb(0, 0, 0);
	font-size: 0.85rem;
	font-weight: 500;
	padding: 0 0.2rem;
	opacity: 0.6;
}
 .bubble-menu button:hover, .bubble-menu button.is-active {
	opacity: 1;
}
</style>