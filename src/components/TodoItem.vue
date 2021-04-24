<template>
  <li>
    <template v-if="isEditing">
      <input type="text" v-model="title" />
      <button @click="handleUpdate">update</button>
    </template>
    <template v-else>
      {{ title }}
      <button @click="handleEdit">edit</button>
      <button @click="handleDelete">delete</button>
    </template>
  </li>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  toRefs,
  useContext,
} from '@nuxtjs/composition-api'

export default defineComponent({
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true,
    },
  },
  setup(props, ctx) {
    const { todo } = toRefs(props)
    // @ts-ignore
    const { $supabase } = useContext()

    const isEditing = ref(false)
    const title = ref(todo.value.title)

    const handleEdit = () => {
      isEditing.value = true
    }

    const handleUpdate = async () => {
      const { error } = await $supabase
        .from('todos')
        .update({ title: title.value })
        .eq('id', todo.value.id)
        .single()

      if (error) {
        alert(error.message)
        return
      }

      isEditing.value = false
    }

    const handleDelete = async () => {
      await $supabase.from('todos').delete().eq('id', todo.value.id).single()
      ctx.emit('fetchTodos')
    }

    return {
      handleDelete,
      handleEdit,
      handleUpdate,
      isEditing,
      title,
    }
  },
})
</script>
