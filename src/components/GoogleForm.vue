<template>
  <iframe
    src="https://docs.google.com/forms/d/e/1FAIpQLSdnYNTAqiGTCL-iKc-6BhZRsdZ6QtmTIDFI8sFBx0I0SzMfdg/viewform?embedded=true"
    :width="screenWidth"
    height="1000"
    frameborder="0"
    marginheight="0"
    marginwidth="0"
  >Loading…</iframe>
</template>

<script>
export default {
  computed: {
    screenWidth() {
      return screen.width;
    }
  }
};
</script>

<style>
</style>