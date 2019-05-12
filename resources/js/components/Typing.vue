<template>
  <div>
    <em>{{ splittedUserNames }} is typing...</em>
  </div>
</template>

<script>
export default {
  props: {
    names: {
      type: Array,
      required: true
    }
  },
  computed: {
    splittedUserNames() {
      return this.names.join(", ");
    }
  }
};
</script>
