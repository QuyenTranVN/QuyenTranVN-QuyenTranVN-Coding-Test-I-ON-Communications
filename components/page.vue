<!-- @format -->
<template>
  <div class="content">
    <div v-for="(text, index) in texts.data" :key="index">
      <div v-if="text.type === 'button'">
        <v-btn @click="alertMessage(text.buttonAlertForm)">{{ text.buttonTextForm }}</v-btn>
      </div>
      <div v-else>
        <p>{{ text.paragraphForm }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({
  props: { texts: { type: Array, default: () => [] } },
  setup() {
    const alertMessage = (message) => {
      alert(message)
    }
    return { alertMessage }
  },
})
</script>

<style lang="scss">
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
