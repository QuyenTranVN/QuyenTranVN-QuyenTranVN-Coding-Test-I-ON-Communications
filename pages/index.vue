<!-- @format -->

<template>
  <div><page :texts="texts" /></div>
</template>
<script>
import { defineComponent, ref, onMounted } from '@nuxtjs/composition-api'
import axios from 'axios'
import page from '~/components/page.vue'
export default defineComponent({
  components: {
    page,
  },
  setup() {
    const texts = ref()

    onMounted(async () => {
      texts.value = await axios.get('http://localhost:3000/posts')
    })

    return { texts }
  },
})
</script>
