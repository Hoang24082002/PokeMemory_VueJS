<template>
  <p class="copyright">
    Copyright by DVHoang
    <a href="#">abcd</a>
  </p>
</template>

<script>
export default {};
</script>

<style lang="css" scoped>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
