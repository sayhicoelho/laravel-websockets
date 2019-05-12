<template>
  <div>
    <strong>{{ message.from }}</strong>
    : {{ message.body }}
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      required: true
    }
  }
};
</script>
