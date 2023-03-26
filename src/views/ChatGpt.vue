<template>
  <v-container>
    <div class="d-flex">
      <v-container class="mx-2">
        <v-form validate-on="submit" @submit.prevent="handleSubmit">
          <v-textarea clearable label="入力してください" variant="outlined" v-model="input"></v-textarea>
          <v-btn type="submit" block class="mt-2">送信</v-btn>
        </v-form>
      </v-container>
      <v-container class="mx-2">
        <div class="container">
          <div v-if="result">
            <div class="view">
              {{ result }}
            </div>
          </div>
          <div v-else>結果がありません</div>
        </div>
      </v-container>
    </div>
  </v-container>
</template>

<script>
import axios from 'axios';

const url = process.env.VUE_APP_AOAI_URL;
const apiKey = process.env.VUE_APP_AOAI_APIKEY;
const headers = {
  'Content-Type': 'application/json',
  'api-key': apiKey,
};
const messageStack = [];
messageStack.push({ role: 'system', content: 'You are a helpful assistant.' });

const data = {
  messages: messageStack
};

export default {
  name: 'ChatGpt',
  components: {},
  data() {
    return {
        input: '',
        result: null,
    };
  },
  computed: {},
  mounted() {},
  methods: {
    async handleSubmit() {
      messageStack.push({ role: 'user', content: this.input });
      this.input = '';
      axios.post(url, data, { headers })
      .then(response => {
        console.log(response.data);
        console.log(response.data.choices[0].message)
        const resJson = response.data.choices[0].message;
        messageStack.push({ role: resJson.role, content: resJson.content });
        console.log(messageStack)
        this.result = response.data;
      })
      .catch(error => {
        console.error(error);
      });
    },
  },
  props: {},
};
</script>
<style scoped>
  .view {
    white-space: pre-wrap;
    word-wrap: break-word;
    text-align: left;
    max-width: 200px;
  }
</style>