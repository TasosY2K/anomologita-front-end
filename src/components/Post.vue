<template>
  <div class="post">
    <p class="user">🧑 {{ info.username }}</p>
    <p class="confession">{{ info.confession }}</p>
    <p class="timestamp">
      ⌛
      <i>{{
        new Date(info.timestamp).toLocaleString("el-GR", {
          timeZone: "Europe/Athens",
        })
      }}</i>
    </p>
  </div>
</template>

<script>
export default {
  name: "Post",
  props: {
    info: Object,
  },
};
</script>

<style scoped>
.confession {
  font-size: 1.3em;
}

.timestamp {
  font-size: 0.8em;
}
</style>
