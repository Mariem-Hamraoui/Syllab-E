<template>
 <the-header></the-header>
  <router-view v-slot= "slotProps">
    <transition name="route" mode="out-in">
      <component :is= "slotProps.Component" ></component>
    </transition>
  </router-view>
</template>

<script>
import TheHeader from "./components/TheHeader.vue";
export default {

  components: {
    TheHeader
  },
   computed: {
    didAutoLogout() {
      return this.$store.getters.didAutoLogout;
    }
   },
    created() {
    this.$store.dispatch('tryLogin');
  },
   watch: {
    didAutoLogout(curValue, oldValue) {
      if (curValue && curValue !== oldValue) {
        this.$router.replace('/dashboard');
      }
    }
  }
}
</script>
