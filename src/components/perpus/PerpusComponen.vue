<script setup>
import { ref, computed } from 'vue'

const daftarBuku = ref([
  {
    judul: 'Laskar Pelangi',
    pengarang: 'Andrea Hirata',
    tahun: 2005,
    img: 'https://upload.wikimedia.org/wikipedia/id/8/8e/Laskar_pelangi_sampul.jpg',
  },
  {
    judul: 'Sang Pemimpi',
    pengarang: 'Andrea Hirata',
    tahun: 2006,
    img: 'https://mmc.tirto.id/image/share/socmed/2019/08/17/poster-sang-pemimpi-imdb_ratio-16x9.jpg',
  },
  {
    judul: 'Edensor',
    pengarang: 'Andrea Hirata',
    tahun: 2007,
    img: 'https://indosastra.com/wp-content/uploads/2019/02/Sinopsis-Novel-Edensor-Andrea-Hirata-200x135.jpg',
  },
  {
    judul: 'Negeri 5 Menara',
    pengarang: 'Ahmad Fuadi',
    tahun: 2009,
    img: 'https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1249749162i/6688121.jpg',
  },
  {
    judul: 'Bumi',
    pengarang: 'Tere Liye',
    tahun: 2014,
    img: 'https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1508413738i/31339471.jpg',
  },
  {
    judul: 'Bulan',
    pengarang: 'Tere Liye',
    tahun: 2015,
    img: 'https://images.tokopedia.net/img/cache/500-square/VqbcmM/2021/8/30/998da4ca-650a-4e09-9489-d44f6d48be76.jpg',
  },
  {
    judul: 'Cantik Itu Luka',
    pengarang: 'Eka Kurniawan',
    tahun: 2002,
    img: 'https://cdn.gramedia.com/uploads/items/9786020366517_Cantik-Itu-Luka-Hard-Cover---Limited-Edition.jpg',
  },
  {
    judul: 'Perahu Kertas',
    pengarang: 'Dee Lestari',
    tahun: 2009,
    img: 'https://inc.mizanstore.com/aassets/img/com_cart/produk/covFP-25.jpg',
  },
  {
    judul: 'Dilan 1990',
    pengarang: 'Pidi Baiq',
    tahun: 2014,
    img: 'https://miro.medium.com/v2/resize:fit:318/1*z7_D-zRx3-owismQrD-MTA.jpeg',
  },
  {
    judul: 'Pulang',
    pengarang: 'Leila S. Chudori',
    tahun: 2012,
    img: 'https://static.dw.com/image/16824693_4.webp',
  },
  {
    judul: 'Geez & Ann',
    pengarang: 'Rintik Sedu',
    tahun: 2018,
    img: 'https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1512718878i/37003233.jpg',
  },
  {
    judul: 'Manusia Setengah Salmon',
    pengarang: 'Raditya Dika',
    tahun: 2011,
    img: 'https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1324711833i/13290213.jpg',
  },
])

const isOpen = ref(false)
const status = ref('Tambah')
const selectedIndex = ref(null)

const form = ref({
  judul: '',
  pengarang: '',
  tahun: '',
  img: '',
})

const keywordJudul = ref('')
const keywordPengarang = ref('')

const filteredBuku = computed(() => {
  return daftarBuku.value.filter(
    (b) =>
      b.judul.toLowerCase().includes(keywordJudul.value.toLowerCase()) &&
      b.pengarang.toLowerCase().includes(keywordPengarang.value.toLowerCase()),
  )
})

const addButton = () => {
  form.value = { judul: '', pengarang: '', tahun: '', img: '' }
  status.value = 'Tambah'
  selectedIndex.value = null
  isOpen.value = true
}

const saveBuku = () => {
  const isFormValid = form.value.judul && form.value.pengarang && form.value.tahun
  if (!isFormValid) {
    alert('Semua kolom wajib diisi!')
    return
  }

  const isDuplicate = daftarBuku.value.some(
    (b, i) =>
      i !== selectedIndex.value &&
      b.judul.toLowerCase() === form.value.judul.toLowerCase() &&
      b.pengarang.toLowerCase() === form.value.pengarang.toLowerCase(),
  )

  if (isDuplicate) {
    alert('Buku dengan judul dan pengarang yang sama sudah ada.')
    return
  }

  if (status.value === 'Tambah') {
    daftarBuku.value.push({ ...form.value })
  } else if (status.value === 'Edit' && selectedIndex.value !== null) {
    daftarBuku.value[selectedIndex.value] = { ...form.value }
  }

  isOpen.value = false
  form.value = { judul: '', pengarang: '', tahun: '', img: '' }
  selectedIndex.value = null
}

const editButton = (buku, index) => {
  form.value = { ...buku }
  status.value = 'Edit'
  selectedIndex.value = index
  isOpen.value = true
}

const deleteBuku = (buku) => {
  const confirmDelete = confirm(`Hapus buku "${buku.judul}"?`)
  if (confirmDelete) {
    const index = daftarBuku.value.findIndex((b) => b.judul === buku.judul)
    if (index !== -1) daftarBuku.value.splice(index, 1)
  }
}
</script>

<template>
  <div class="p-6">
    <h1 class="text-3xl font-bold text-center text-[#3C3C3C] mb-6">data perpustakaan</h1>

    <!-- Filter -->
    <div class="flex gap-4 mb-6 justify-center">
      <input
        type="text"
        placeholder="Cari berdasarkan judul"
        v-model="keywordJudul"
        class="border border-gray-300 rounded-md px-4 py-2 w-1/3"
      />
      <input
        type="text"
        placeholder="Cari berdasarkan pengarang"
        v-model="keywordPengarang"
        class="border border-gray-300 rounded-md px-4 py-2 w-1/3"
      />
      <button
        class="bg-[#3C3C3C] text-white w-auto p-3 h-auto rounded-lg shadow-lg text-[12px] font-semibold"
        @click="addButton"
      >
        tambah Buku
      </button>
    </div>

    <!-- Modal Form -->
    <div v-if="isOpen" class="fixed inset-0 bg-[#28282880] flex justify-center items-center z-50">
      <div class="bg-white p-6 rounded-lg shadow-lg w-full max-w-xl relative">
        <button class="absolute top-2 right-3 text-gray-500 text-2xl" @click="isOpen = false">
          &times;
        </button>
        <h2 class="text-2xl font-bold mb-4">{{ status }} Buku</h2>

        <form class="flex flex-col gap-4" @submit.prevent="saveBuku">
          <input
            type="text"
            v-model="form.judul"
            placeholder="Judul Buku"
            class="border border-gray-300 rounded-md px-4 py-2"
          />
          <input
            type="text"
            v-model="form.pengarang"
            placeholder="Nama Pengarang"
            class="border border-gray-300 rounded-md px-4 py-2"
          />
          <input
            type="number"
            v-model="form.tahun"
            placeholder="Tahun Terbit"
            class="border border-gray-300 rounded-md px-4 py-2"
          />
          <input
            type="text"
            v-model="form.img"
            placeholder="URL Gambar Cover"
            class="border border-gray-300 rounded-md px-4 py-2"
          />
          <button
            type="submit"
            class="bg-[#3C3C3C] text-white px-4 py-2 rounded hover:bg-[#3C3C3C80]"
          >
            Simpan Buku
          </button>
        </form>
      </div>
    </div>

    <!-- Daftar Buku -->
    <div class="flex flex-wrap gap-6 justify-center">
      <div
        v-for="(buku, index) in filteredBuku"
        :key="index"
        class="w-80 border shadow rounded-lg p-4 bg-[#3C3C3C]"
      >
        <img :src="buku.img" :alt="buku.judul" class="w-full h-48 mb-3 rounded-lg" />
        <h2 class="text-xl text-white font-semibold mb-2">{{ buku.judul }}</h2>
        <p class="text-white mb-1">
          Pengarang: <strong>{{ buku.pengarang }}</strong>
        </p>
        <p class="text-white mb-4">Tahun: {{ buku.tahun }}</p>
        <div class="flex justify-between">
          <button
            class="bg-[#cbcbcb] text-[#3C3C3C] px-4 py-1 rounded"
            @click="editButton(buku, index)"
          >
            Edit
          </button>
          <button class="bg-red-500 text-white px-4 py-1 rounded" @click="deleteBuku(buku)">
            Hapus
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
