<template>
  <div class="row">
    <div class="col-md-4">
      <sidebar v-bind:users="users"/>
    </div>
    <div class="col-md-8">
      <chat
        v-bind:messages="messages"
        v-bind:whos-typing="whosTyping"
        v-on:message-sent="pushMessage"
        v-on:typing="whisper"
      />
    </div>
  </div>
</template>

<script>
import Chat from "./Chat";
import Sidebar from "./Sidebar";

export default {
  components: {
    Chat,
    Sidebar
  },
  data() {
    return {
      channel: null,
      users: [],
      messages: [],
      whosTyping: []
    };
  },
  methods: {
    whisper(isTyping) {
      this.channel.whisper("typing", {
        from: Laravel.user.name,
        typing: isTyping
      });
    },
    pushMessage(message) {
      this.messages.push(message);
    },
    pushUser(user) {
      this.users.push(user);
    },
    pushWhosTyping(name) {
      this.whosTyping.push(name);
    },
    removeTyping(name) {
      this.whosTyping = this.whosTyping.filter(n => n !== name);
    },
    removeUser(user) {
      this.users = this.users.filter(u => u.id !== user.id);
    }
  },
  mounted() {
    this.channel = Echo.join("Chat")
      .here(users => {
        this.users = users;
      })
      .joining(user => {
        this.pushUser(user);
      })
      .leaving(user => {
        this.removeUser(user);
      })
      .listen("MessageSent", e => {
        this.pushMessage(e.message);
      })
      .listenForWhisper("typing", e => {
        if (e.typing) this.pushWhosTyping(e.from);
        else this.removeTyping(e.from);
      });
  }
};
</script>
