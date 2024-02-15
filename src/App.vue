<template>
  <main class="mx-auto max-w-screen-md space-y-3 p-3">
    <header>
      <h1 class="text-2xl font-header">Products</h1>
    </header>
    <VCard>
      <template #content>
        <VList :items="filteredItems">
          <template #header>
            <div class="flex flex-row items-center justify-between w-full space-x-2">
              <input
                type="text"
                placeholder="Find a Product"
                class="input flex-1"
                v-model="filters.search"
              >
              <button class="btn btn-primary">
                <span>
                  Search
                </span>
              </button>
            </div>
            <div class="flex flex-row items-center justify-between w-full space-x-2">
              <input
                type="text"
                placeholder="Product name"
                class="input flex-1"
                v-model="newProductName"
                @keyup.enter="addProduct"
              >
              <button class="btn btn-secondary" @click="addProduct">
                <span>
                  Add Product
                </span>
              </button>
            </div>
          </template>
          <template #item="{ key, index, item }">
            <VListItem
              :key="key"
              :item="item"
              @replace-image="replaceImage(index, $event)"
              @toggle-status="toggleStatus(index, $event)"
            />
          </template>
        </VList>
      </template>
    </VCard>
    <footer class="flex flex-row items-center justify-between">
      <div>
        <span>Page <strong class="font-bold">1</strong> of <strong class="font-bold">{{ totalPages }}</strong></span>
      </div>
      <div class="flex flex-row items-center space-x-2">
        <span>Sort by</span>
        <select class="px-2 py-1 rounded-md" v-model="filters.sort">
          <option disabled value="">Sort Order</option>
          <option :value="option" :key="option" v-for="option in sortOptions">{{ option }}</option>
        </select>
      </div>
      <select class="px-2 py-1 rounded-md" v-model="filters.itemsPerPage">
        <option :value="option" :key="option" v-for="option in itemsPerPageOptions">{{ option }}</option>
      </select>
    </footer>
  </main>
</template>

<script>
import VCard from './components/VCard.vue';
import VList from './components/VList.vue';
import VListItem from './components/VListItem.vue';

export default {
  components: {
    VCard,
    VList,
    VListItem,
  },
  data() {
    return {
      sortOptions: [
        'A-Z',
        'Last Modified',
        'Created Date',
      ],
      itemsPerPageOptions: [
        10,
        25,
        50,
        100
      ],
      filters: {
        sort: '',
        itemsPerPage: 10,
        search: '',
      },
      newProductName: '',
      items: [
        {
          name: 'Americano',
          image: 'https://picsum.photos/48',
          status: 'green',
          date: null,
        },
        {
          name: 'Espresso',
          image: 'https://picsum.photos/49',
          status: 'yellow',
          date: null,
        },
        {
          name: 'Mocha',
          image: '',
          status: 'green',
          date: null,
        },
        {
          name: 'White Mocha',
          image: '',
          status: 'yellow',
          date: 704305433,
        },
        {
          name: 'Latte',
          image: 'https://picsum.photos/12',
          status: 'green',
          date: 1666715033,
        },
        {
          name: 'Cappucino',
          image: 'https://picsum.photos/33',
          status: 'green',
          date: null,
        },
        {
          name: 'Walking with Giants',
          image: 'https://picsum.photos/64',
          status: 'green',
          date: null,
        },
        {
          name: 'Turbinator 2',
          image: 'https://picsum.photos/87',
          status: 'red',
          date: null,
        },
        {
          name: 'Super Cali Fragilistic Expialidocious',
          image: '',
          status: 'red',
          date: 704305433,
        },
        {
          name: 'Baseball Hat',
          image: 'https://picsum.photos/78',
          status: 'green',
          date: null,
        },
        {
          name: 'Americano',
          image: 'https://picsum.photos/46',
          status: 'green',
          date: null,
        },
        {
          name: 'Espresso',
          image: 'https://picsum.photos/99',
          status: 'yellow',
          date: null,
        },
        {
          name: 'Mocha',
          image: '',
          status: 'green',
          date: null,
        },
        {
          name: 'White Mocha',
          image: '',
          status: 'yellow',
          date: 704305433,
        },
        {
          name: 'Latte',
          image: 'https://picsum.photos/43',
          status: 'green',
          date: 1666715033,
        },
        {
          name: 'Cappucino',
          image: 'https://picsum.photos/75',
          status: 'green',
          date: null,
        },
        {
          name: 'Walking with Giants',
          image: 'https://picsum.photos/97',
          status: 'green',
          date: null,
        },
        {
          name: 'Turbinator 2',
          image: 'https://picsum.photos/30',
          status: 'red',
          date: null,
        },
        {
          name: 'Super Cali Fragilistic Expialidocious',
          image: '',
          status: 'red',
          date: 704305433,
        },
        {
          name: 'Baseball Hat',
          image: 'https://picsum.photos/74',
          status: 'green',
          date: null,
        },
      ],
    }
  },
  computed: {
    filteredItems() {
      let sortedItems = this.items.map((item) => ({...item}))
      if (this.filters.sort) {
        switch (this.filters.sort) {
          case 'A-Z':
            sortedItems = sortedItems.sort((a, b) => {
              if (a.name < b.name) {
                return -1
              }
              if (a.name > b.name) {
                return 1
              }
              return 0
            })
            break;
          case 'Last Modified':
          case 'Created Date':
            sortedItems = sortedItems.sort((a, b) => new Date(b.date) - new Date(a.date))
            break;
        }
      }
      if (this.filters.search) {
        return sortedItems.filter((item) => item.name.toUpperCase().includes(this.filters.search.toUpperCase()))
      }
      return sortedItems.slice(0, this.filters.itemsPerPage)
    },
    totalPages() {
      return Math.ceil(this.items.length / this.filters.itemsPerPage)
    },
  },
  methods: {
    addProduct() {
      const imageCode = Math.floor(Math.random() * (80 - 10))
      this.items.push({
        name: this.newProductName,
        image: `https://picsum.photos/${imageCode}`,
        status: 'yellow',
        date: Date.now()
      })
      this.newProductName = ''
    },
    replaceImage(index, imageCode) {
      if (imageCode) {
        this.items[index].image = `https://picsum.photos/${imageCode}`
      } else {
        this.items[index].image = null
      }
    },
    toggleStatus(index, status) {
      this.items[index].status = status
    }
  }
}
</script>
