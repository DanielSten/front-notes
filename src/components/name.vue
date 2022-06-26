<template>
  <li>{{ output }}</li>
</template>

<script>
export default {
  name: "name",
  props: {
    output: '',
  },
}
</script>

<style scoped>
  li{
    font-size: 23px;
    padding: 5px 15px;
    border: 1px solid #1f1f1f;
    border-radius: 3px;
    background-color: #83e1ef;
    margin-top: 5px;
  }
</style>