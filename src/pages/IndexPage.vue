<template>
  <q-page padding>
    <q-table
          title="Treats"
          :rows="posts"
          :columns="columns"
          row-key="name"
        />
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import { api } from 'boot/axios'

export default defineComponent({
  name: 'IndexPage',
  setup () {
    const posts = ref([])
    const columns = [
      { name: 'id', field: 'id', label: 'Id', sortable: true, align: 'left' },
      { name: 'title', field: 'title', label: 'Title', sortable: true, align: 'left' },
      { name: 'author', field: 'author', label: 'Author', sortable: true, align: 'left' }
    ]

    onMounted(() => {
      getPosts()
    })

    const getPosts = async () => {
      try {
        const { data } = await api.get('posts')
        posts.value = data
        console.log(posts.value)
      } catch (error) {
        console.error(error)
      }
    }
    return {
      posts,
      columns
    }
  }
})
</script>
