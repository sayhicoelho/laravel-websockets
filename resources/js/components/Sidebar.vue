<template>
  <div>
    <h2>Users</h2>
    <ul class="list-group mb-3">
      <li class="list-group-item" v-for="(user, index) in users" v-bind:key="index">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    users: {
      type: Array,
      required: true
    }
  }
};
</script>
