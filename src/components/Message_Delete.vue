<template>
  <div class="message-container">
    <p>{{ msg }}</p>
  </div>
</template>

<script>
export default {
  name: "MessageDelete",
  props: {
    msg: String,
  },
};
</script>

<style scoped>
.message-container{
    color: #580000;
    background-color: #ffcccc;
    border: 2px solid #ffb8b8;
    border-radius: 5px;
    padding: 10px;
    max-width: 400px;
    margin: 30px auto;
}
</style>