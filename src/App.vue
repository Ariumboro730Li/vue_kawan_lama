<template>
  <div class="container">
    <the-parent></the-parent>
  </div>
</template>

<script>
import TheParent from "./components/Parent.vue";

export default {
  name: 'App',
  components: {
    TheParent,
  }
}
</script>
