<template>
  <div :class="{ 'message': true, 'sent': isSent }">
    <div class="message-content">{{ message }}</div>
  </div>
</template>

<script>
export default {
  props: {
    message: String,
    isSent: Boolean,
  }
}
</script>
<style scoped>
.message {
  display: flex;
  margin: 5px 0;
  padding: 5px;
  border-radius: 5px;
  max-width: 80%;
  min-width: 60%;
  word-break: break-word;
  background-color: antiquewhite;
}

.sent {
  align-self: flex-end;
  background-color: #4CAF50;
  color: white;
  max-width: 80%;
  min-width: 60%;
}

.received {
  align-self: flex-start;
  background-color: #f1f1f1;
}
</style>