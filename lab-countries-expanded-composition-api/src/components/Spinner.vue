<template>
  <div class="d-flex flex-column justify-center">
    <div class="spinner-border mb-5" role="status">
      <span class="sr-only"></span>
    </div>
    <p class="text-center">{{ text }}</p>
  </div>
</template>

<script>
export default {
  name: "Spinner",
  props: {
    text: String,
  },
};
</script>

<style scoped></style>
