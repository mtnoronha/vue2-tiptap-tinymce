# Integration with Vue

- With TipTap we need to use [3rd party components](https://github.com/Leecason/element-tiptap) or create one ourself.
- With TinyMCE we have an [Official Vue Integration](https://github.com/tinymce/tinymce-vue).

# Paste formatted documents (like word etc)

[Tiny](doc-TinyMCE.png)
vs
[TipTap](doc-tiptap.png)

I found this project called [Element TipTap](https://github.com/Leecason/element-tiptap) that wraps everything we need, and tested pasted the same document for a reference. But there's [this bug](https://github.com/Leecason/element-tiptap/issues/170) that prevents us to use FontSize and paste documents without crashing.


