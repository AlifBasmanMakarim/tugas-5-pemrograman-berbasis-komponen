<script setup>
import { ref, computed, watch, onMounted } from 'vue';

const menus = ref([
  { id: 1, name: 'Nasi Goreng', type: 'Makanan' },
  { id: 2, name: 'Ayam Geprek', type: 'Makanan' },
  { id: 3, name: 'Teh Es', type: 'Minuman' },
  { id: 4, name: 'Milo', type: 'Minuman' }
]);

const namaMenu = ref('');
const kategoriMenu = ref('Makanan');

// Template Ref untuk input
const inputNamaMenu = ref(null);

// Lifecycle Hook: Fokus ke input saat pertama kali halaman dimuat
onMounted(() => {
  if (inputNamaMenu.value) {
    inputNamaMenu.value.focus();
  }
});

// Computed properties untuk memisahkan menu makanan dan minuman
const menuMakanan = computed(() => menus.value.filter(menu => menu.type === 'Makanan'));
const menuMinuman = computed(() => menus.value.filter(menu => menu.type === 'Minuman'));

const tambahMenu = () => {
  if (namaMenu.value.trim()) {
    menus.value.push({
      id: menus.value.length + 1,
      name: namaMenu.value,
      type: kategoriMenu.value
    });
    namaMenu.value = '';

    // Fokus kembali ke input setelah menambahkan menu
    inputNamaMenu.value.focus();
  }
};

// Watchers untuk melihat perubahan input
watch(namaMenu, (newValue, oldValue) => {
  console.log(`Nama menu berubah dari "${oldValue}" ke "${newValue}"`);
});

watch(kategoriMenu, (newValue, oldValue) => {
  console.log(`Kategori menu berubah dari "${oldValue}" ke "${newValue}"`);
});
</script>

<template>
  <div>
    <div>
      <label><strong>Tambah Menu</strong></label>
      <input ref="inputNamaMenu" v-model="namaMenu" placeholder="Nama menu" />
      <select v-model="kategoriMenu">
        <option>Makanan</option>
        <option>Minuman</option>
      </select>
      <button @click="tambahMenu">Kirim</button>
    </div>
    <hr />
    <h1>Daftar Menu Tersedia:</h1>
    <ul>
      <li v-for="menu in menus" :key="menu.id">
        {{ menu.id }} - {{ menu.name }} - {{ menu.type }}
      </li>
    </ul>

    <h2>Menu Makanan</h2>
    <ul>
      <li v-for="menu in menuMakanan" :key="menu.id">
        {{ menu.id }} - {{ menu.name }} - {{ menu.type }}
      </li>
    </ul>

    <h2>Menu Minuman</h2>
    <ul>
      <li v-for="menu in menuMinuman" :key="menu.id">
        {{ menu.id }} - {{ menu.name }} - {{ menu.type }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
input, select, button {
  margin: 5px;
  padding: 5px;
}
button {
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
  padding: 5px 10px;
}
button:hover {
  background-color: #369f76;
}
h1 {
  font-size: 24px;
  font-weight: bold;
}
h2 {
  font-size: 20px;
  font-weight: bold;
  margin-top: 10px;
}
</style>
