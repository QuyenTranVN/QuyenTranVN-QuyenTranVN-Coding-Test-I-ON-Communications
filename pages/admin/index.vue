<!-- @format -->
<template>
  <div><MainPage /></div>
</template>
<script>
import { defineComponent } from '@nuxtjs/composition-api'
import MainPage from '~/components/admin/MainPage.vue'
export default defineComponent({
  components: {
    MainPage,
  },
  setup() {
    return {}
  },
})
</script>
