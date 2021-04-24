<template>
  <CBox>
    <Header />
    <CHeading align="center">TODO</CHeading>
    <template v-if="!$fetchState.pending">
      <ul>
        <TodoItem v-for="todo in todos" :key="todo.id" :todo="todo" />
      </ul>
    </template>
  </CBox>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  useContext,
  useFetch,
} from '@nuxtjs/composition-api'
import { CBox, CHeading } from '@chakra-ui/vue'
import Header from '@/components/Header.vue'
import TodoItem from '@/components/TodoItem.vue'

export default defineComponent({
  name: 'App',
  components: {
    CBox,
    CHeading,
    Header,
    TodoItem,
  },
  setup() {
    const todos = ref([])
    // @ts-ignore
    const { $supabase } = useContext()

    const sleep = (millisec: number) => {
      return new Promise((resolve) => {
        setTimeout(resolve, millisec)
      })
    }

    const { fetch, fetchState } = useFetch(async () => {
      const res = await $supabase.from('todos').select('id, title').order('id')
      todos.value = res.body
    })

    fetch()

    return { todos }
  },
})
</script>
