<template>
  <div id="app">
    This is TipTap:
    <TipTap v-model="text"/>

    <br><br>
    This is TinyMCE:
    <TinyMce v-model="text2" />
    <br><br>
    These are their results:
    <hr>
    {{ text }}
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
