<template>
  <CBox>
    <Header />
    <CHeading align="center">TODO</CHeading>
    <template v-if="!$fetchState.pending">
      <ul>
        <TodoItem
          v-for="todo in todos"
          :key="todo.id"
          :todo="todo"
          @fetchTodos="$fetch"
        />
      </ul>
    </template>
    <input type="text" v-model="newTitle" />
    <button @click="addTodo" :disabled="!newTitle">Add</button>
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
    const newTitle = ref('')

    // @ts-ignore
    const { $supabase } = useContext()

    const { fetch } = useFetch(async () => {
      const { data, error } = await $supabase
        .from('todos')
        .select('*')
        .order('id')

      if (error) {
        alert(error)
        return
      }

      todos.value = data
    })

    fetch()

    const addTodo = async () => {
      const { error } = await $supabase
        .from('todos')
        .insert([{ title: newTitle.value }])

      if (error) {
        alert(error.message)
        return
      }
      fetch()
      newTitle.value = ''
    }

    return { addTodo, newTitle, todos }
  },
})
</script>
