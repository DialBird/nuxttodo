<template>
  <CBox>
    <Header />
    <CHeading align="center">TODO</CHeading>
    <ul>
      <TodoItem v-for="todo in todos" :key="todo.id" :todo="todo" />
    </ul>
  </CBox>
</template>

<script lang="js">
import { CBox, CHeading } from '@chakra-ui/vue'
import Header from '@/components/Header'
import TodoItem from '@/components/TodoItem'

export default {
  name: 'App',
  components: {
    CBox,
    CHeading,
    Header,
    TodoItem,
  },
  inject: ['$chakraColorMode', '$toggleColorMode'],
  data () {
    return {
      showModal: false,
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        }
      },
      todos: []
    }
  },
  mounted() {
    this.$supabase.from('todos').select('id, title').order('id').then(res => {
      this.todos = res.body
    })
  },
  computed: {
    colorMode () {
      return this.$chakraColorMode()
    },
    theme () {
      return this.$chakraTheme()
    },
    toggleColorMode () {
      return this.$toggleColorMode
    }
  },
  methods: {
    showToast () {
      this.$toast({
        title: 'Account created.',
        description: "We've created your account for you.",
        status: 'success',
        duration: 10000,
        isClosable: true
      })
    }
  }
}
</script>
