<template>
<v-container>
    <div class="d-flex">
        <v-container class="mx-2">
            <v-form validate-on="submit" @submit.prevent="submit">
                <v-textarea clearable label="markdown" variant="outlined" v-model="md"></v-textarea>
                <v-btn type="submit" block class="mt-2">表示</v-btn>
            </v-form>
        </v-container>
        <v-container class="mx-2">
            <div class="container">
                <vue-markdown-it :source="source" />
            </div>
        </v-container>
    </div>
</v-container>

</template>

<script>
import VueMarkdownIt from 'vue3-markdown-it';
import 'highlight.js/styles/monokai.css';

export default {
    name: "MarkdownView",
    components: {
        VueMarkdownIt,
    },
    data() {
        return {
            md: "# Hello World",
            source: '',
        }
    },
    methods: {
        submit() {
            console.log(this.md)
            this.source = this.md
        }
    }
}
</script>
<style scoped>
.container {
  text-align: left;
}
</style>