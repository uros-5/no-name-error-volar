<template>
  <div id="roundchat" class="roundchat chat">
    <div class="chatroom">
      Chat room
      <input
        id="checkbox"
        title="Toggle the chat"
        name="checkbox"
        type="checkbox"
        @click="toggle"
      />
    </div>
    <ol id="lobbychat-messages">
      <div id="messages" v-if="hiddenChat">
        <li v-for="i in messages" :key="i" class="message">
          <div class="time">{{ i.time }}</div>
          <span class="user">
            <a href="/">{{ i.user }}</a>
          </span>
          <span> {{ i.message }} </span>
        </li>
      </div>
    </ol>

    <input
      v-model="message"
      v-on:keyup.enter="onEnter"
      id="chat-entry"
      maxlength="140"
      type="text"
      name="entry"
      autocomplete="off"
      :placeholder="setPlaceholder()"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, Ref } from "vue";

const props = defineProps({ messages: [], wsType: String });
const messages: Ref<ChatMessage[]> = ref(props.messages!);
const message = ref("");
const hiddenChat = ref(true);

function onEnter(): void {
  if (message.value.length > 0 && message.value.length < 80) {
    let obj: ChatMessage = {
      time: getTime(),
      user: "Abc",
      message: message.value,
    };
    messages.value.push(obj);
    //ws.send...
    message.value = "";
  }
}

function setPlaceholder(): string {
  let reg: boolean = true;
  if (!reg) {
    return "Sign in to chat";
  } else {
    return "Please be nice in the chat!";
  }
}

function toggle(): void {
  hiddenChat.value = !hiddenChat.value;
}

function getTime(): string {
  let time = new Date();
  return `${time.getHours()}:${time.getMinutes()}`;
}

interface ChatMessage {
  time: String;
  user: String;
  message: String;
}
</script>

<style scoped>
.user {
  padding-right: 6px;
  font-weight: bold;
}
</style>
