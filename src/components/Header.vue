<template>
  <c-box border-bottom="2px" mb="4" border-color="tomato" box-shadow="lg">
    <c-flex p="4"> Header </c-flex>
  </c-box>
</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'
import { CBox, CFlex } from '@chakra-ui/vue'

export default defineComponent({
  name: 'Header',
  components: {
    CBox,
    CFlex,
  },
  setup() {},
})
</script>
