<template>
    <div class="chatgpt">
      <h1>This is an ChatGPT page</h1>
    </div>
    <div>
    <form @submit.prevent="handleSubmit">
      <label for="input">入力してください:</label>
      <input type="text" v-model="input" id="input">
      <button type="submit">送信</button>
    </form>
    <div v-if="result">{{ result }}</div>
    <div v-else>結果がありません</div>
  </div>

  <div class="container">
        <vue-markdown-it :source="source" />
    </div>
    <div class="chat">
        <div class="messages">
          <chatMessages v-for="message in messages" :key="message.id" :message="message.text" :is-sent="message.isSent" />
        </div>
        <div class="input">
          <input v-model="newMessage" @keyup.enter="sendMessage" type="text" placeholder="Type your message here..." />
        </div>
    </div>
</template>
<script>
import VueMarkdownIt from 'vue3-markdown-it';
import 'highlight.js/styles/monokai.css';
import ChatMessages from '@/components/ChatMessages.vue';
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
    VueMarkdownIt,
    ChatMessages,
  },
  data() {
    return {
        source: source,
        messages: [
          { id: 1, text: 'Hi there!', isSent: false },
          { id: 2, text: 'Hey!', isSent: true },
        ],
        newMessage: '',
        nextMessageId: 3,
        input: '',
        result: null,
    };
  },
  computed: {},
  mounted() {},
  methods: {
    sendMessage() {
        if(!this.newMessage){
            return;
        }
        this.messages.push({
            id: this.nextMessageId ++,
            text: this.newMessage,
            isSent: true,
        });
        this.newMessage = '';
    },
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

.chat {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.messages {
  flex: 1;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  padding: 10px;
}

.input {
  display: flex;
  align-items: center;
  padding: 10px;
}

input[type="text"] {
  flex: 1;
  margin: 5px;
  padding: 10px;
  border-radius: 5px;
  border: none;
  box-shadow: 0 0 5px rgba(0, 0, 00.1);
  font-size: 16px;
}
</style>