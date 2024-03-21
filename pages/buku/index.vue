<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">Cari Buku</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau baca apa hari ini?">
          </form>
        </div>
        <div class="my-3 text-muted">menampilkan 3 dari 3</div>
        <div class="row">
          <div v-for="(book, i) in books" :key="i" class="col-lg-2">
            <nuxt-link :to="`/buku/${book.id}`">
              <div class="card mb-3">
                <div class="card-body">
                  <h3>{{ book.judul }}</h3>
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
    <nuxt-link to="/">
      <button type="button" class="btn btn-dark">Kembali</button>
    </nuxt-link>
  </div>
</template>

<style scoped>
.card-body {
  padding: 0;
}

.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

button {
  width: 100px;
}

img {
  width: 100%;
}
</style>


<script setup>
const supabase = useSupabaseClient()

const books = ref([])
const keyword = ref('')

const getBooks = async () => {
  const { data, error } = await supabase.from('Buku').select(`*, kategori(*)`)
    .ilike(`judul`, `%${keyword.value}`)
  if (data) books.value = data
}

onMounted(() => {
  getBooks()
})
</script>