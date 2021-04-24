<template>
  <li>
    <template v-if="isEditing">
      <input type="text" v-model="title" />
      <button @click="handleUpdate">update</button>
    </template>
    <template v-else>
      {{ title }}
      <button @click="handleEdit">edit</button>
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
        console.error('There was an error updating', error)
        return
      }

      console.log('Updated task', todo.value.id, title.value)
      isEditing.value = false
    }

    return {
      handleEdit,
      handleUpdate,
      isEditing,
      title,
    }
  },
})
</script>
