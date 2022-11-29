<template>
  <div id="app">
    Teste do TipTap:
    <TipTap v-model="text"/>

    <hr>
    {{ text }}
    <hr>
    <TinyMce v-model="text2" />
    <hr>
    {{ text2 }}
    <hr>
  </div>
</template>

<script>
import TipTap from './components/TipTap.vue';
import TinyMce from './components/TinyMce.vue';

export default {
  name: 'App',
  data() {
    return {
      text: 'TipTap empty for now...',
      text2: 'TinyMce empty for now...',
    };
  },
  components: {
    TipTap, TinyMce
  },
  methods: {
  },
}
</script>
<style scoped>
  hr {
    margin-top: 50px;
    margin-bottom: 20px;
  }
</style>
