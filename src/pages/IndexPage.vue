<template>
  <q-page padding>
    <q-table title="Treats" :rows="posts" :columns="columns" row-key="name">
      <template v-slot:body-cell-actions="props">
        <q-td :props="props">
          <q-btn icon="delete" color="negative" dense size="sm" @click="handleDeletePost(props.row.id)" />
        </q-td>
      </template>
    </q-table>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import postsService from 'src/services/posts'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'IndexPage',
  setup () {
    const posts = ref([])
    const { list, remove } = postsService()
    const columns = [
      { name: 'id', field: 'id', label: 'Id', sortable: true, align: 'left' },
      { name: 'title', field: 'title', label: 'Title', sortable: true, align: 'left' },
      { name: 'author', field: 'author', label: 'Author', sortable: true, align: 'left' },
      { name: 'actions', field: 'actions', label: 'Acões', align: 'right' }
    ]

    const $q = useQuasar()
    onMounted(() => {
      getPosts()
    })

    const getPosts = async () => {
      try {
        const data = await list()
        posts.value = data
        console.log(posts.value)
      } catch (error) {
        console.error(error)
      }
    }

    const handleDeletePost = async (id) => {
      try {
        $q.dialog({
          title: 'Atenção',
          message: 'Deseja realmente deletar'
        }).onOk(async () => {
          try {
            await remove(id)
            $q.notify({ message: 'Sucesso', icon: 'check', color: 'positive' })
            await getPosts()
          } catch (error) {
            $q.notify({ message: 'Falha', icon: 'times', color: 'nagative' })
          }
        })
      } catch (error) {
        $q.notify({ message: 'Falha', icon: 'times', color: 'nagative' })
      }
    }
    return {
      posts,
      columns,
      handleDeletePost
    }
  }
})
</script>
