<template>
  <div class="overflow-auto mb-3 messages">
    <ul class="list-group" v-if="hasMessage">
      <li class="list-group-item" v-for="(message, index) in messages" v-bind:key="index">
        <message v-bind:message="message"/>
      </li>
    </ul>
  </div>
</template>

<script>
import Message from "./Message";

export default {
  components: { Message },
  props: {
    messages: {
      type: Array,
      required: true
    }
  },
  computed: {
    hasMessage() {
      return this.messages.length > 0;
    }
  }
};
</script>

<style scoped>
.messages {
  max-height: 300px;
}

@media (min-width: 768px) {
  .messages {
    max-height: 500px;
  }
}
</style>
