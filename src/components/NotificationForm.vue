<template>
  <div class="notification">
    <img src="../assets/success.svg" alt="" />
    <p>Новый клиент успешно создан!</p>
  </div>
</template>

<script>
export default {
  name: "NotificationForm",
};
</script>

<style scoped lang="sass">
.notification
  max-width: 1000px
  padding: 20px
  text-align: center
  p
    font-size: 24px
    font-weight: 500
</style>
