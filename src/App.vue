<template>
  <div id="app">
    <p class="client-form" @click="isCreatClient = false" v-if="isCreatClient">
      Форма создания клиента
    </p>
    <CreationClientForm
      v-if="!isCreatClient"
      @createdClient="isCreatClient = true"
    />
    <NotificationForm class="notification" v-if="isCreatClient" />
  </div>
</template>

<script>
import CreationClientForm from "./components/CreationClientForm.vue";
import NotificationForm from "./components/NotificationForm.vue";

export default {
  name: "App",
  components: {
    CreationClientForm,
    NotificationForm,
  },
  data() {
    return {
      isCreatClient: false,
    };
  },
};
</script>

<style scoped lang="sass">
$firstColor: #00a4af

.client-form
  text-decoration: underline
  cursor: pointer
  &:hover
    color: $firstColor
</style>
