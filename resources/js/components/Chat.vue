<template>
  <div>
    <h2>Chat</h2>
    <messages v-bind:messages="messages"/>
    <typing v-if="hasUserTyping" v-bind:names="whosTyping" class="mb-1"/>

    <input
      type="text"
      v-model="message"
      class="form-control"
      placeholder="Type something..."
      @keydown.enter="sendMessage"
      @input="whisper"
    >

    <button class="btn btn-primary mt-3" @click="sendMessage">Send message</button>
  </div>
</template>

<script>
import Messages from "./Messages";
import Typing from "./Typing";
import { setTimeout, clearTimeout } from "timers";

export default {
  components: {
    Messages,
    Typing
  },
  props: {
    messages: {
      type: Array,
      required: true
    },
    whosTyping: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      message: "",
      typing: false,
      typingTimeout: null
    };
  },
  computed: {
    hasUserTyping() {
      return this.whosTyping.length > 0;
    }
  },
  methods: {
    pushMessage(message) {
      this.$emit("message-sent", message);
    },
    clearMessage() {
      this.message = "";
    },
    canSendMessage(message) {
      return message.length > 0;
    },
    whisper() {
      if (!this.typing) this.sendTyping(true);

      clearTimeout(this.typingTimeout);

      this.typingTimeout = setTimeout(() => {
        this.sendTyping(false);
      }, 1000);
    },
    sendTyping(isTyping) {
      this.$emit("typing", isTyping);
      this.typing = isTyping;
    },
    sendMessage() {
      const { message } = this;

      if (this.canSendMessage(message)) {
        const payload = {
          from: Laravel.user.name,
          body: message
        };

        this.pushMessage(payload);
        this.clearMessage();

        axios.post("sendMessage", { message }).then(response => {
          //
        });
      }
    }
  },
  mounted() {
    // TODO: Get latest messages.
  }
};
</script>
