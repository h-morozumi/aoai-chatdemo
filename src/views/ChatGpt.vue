<template>
    <div class="chatgpt">
      <h1>This is an ChatGPT page</h1>
    </div>
    <div class="container">
        <vue-markdown-it :source="source" />
    </div>
</template>
<script>
import VueMarkdownIt from 'vue3-markdown-it';
import 'highlight.js/styles/monokai.css';

const source = `
# Hello

- apple
- banana

~~打ち消し~~

> aaaaaa
> bbbbb
> cccc

[Qiita](http://qiita.com)
`

export default {
  name: 'ChatGpt',
  components: {
    VueMarkdownIt
  },
  data() {
    return {
        source: source
    };
  },
  computed: {},
  mounted() {},
  methods: {},
};
</script>
<style>
.container {
  margin: 0 auto;
  margin-top: 200px;
  min-height: 100vh;
  width: 500px;
  justify-content: center;
  align-items: center;
  text-align: left;
  background-color: #c0c0c0;
}
</style>