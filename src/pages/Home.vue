<script setup>
import { onMounted, reactive, ref, watch } from 'vue'
import Card from '@/components/Card.vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

const selectedMenuIndex = ref(1)
const items = ref([])
const items2 = ref([
  {
    'id': 1,
    'title': 'Ролл ясай',
    'weight': 185,
    'description': 'Огурец, авокадо, помидор, болгарский перец, листья салата, кунжут',
    'price': 200,
    'imageUrl': '/rolls/roll-yasai.png'
  }, {
    'id': 2,
    'title': 'Мясные роллы',
    'weight': 260,
    'description': 'Огурец, авокадо, помидор, болгарский перец, листья салата, кунжут',
    'price': 240,
    'imageUrl': '/rolls/roll-meat.png'
  }, {
    'id': 3,
    'title': 'Ролл лайт',
    'weight': 190,
    'description': 'Листья салата, огурец, перец, помидор, сырный соус, чипсы',
    'price': 165,
    'imageUrl': '/rolls/roll-light.png'
  }, {
    'id': 4,
    'title': 'Ролл веган',
    'weight': 230,
    'description': 'Листья салата, авокадо, огурец, спаржа, сивид чука, кунжут',
    'price': 200,
    'imageUrl': '/rolls/roll-vegan.png'
  }, {
    'id': 5,
    'title': 'Ролл чиро',
    'weight': 210,
    'description': 'Листья салата, копченая курица, болгарский перец, твердый сыр, чесночный соус',
    'price': 210,
    'imageUrl': '/rolls/roll-chiro.png'
  }, {
    'id': 6,
    'title': 'Ролл тайол',
    'weight': 215,
    'description': 'Огурец, помидор, сыр творожный, бекон, сыр маасдам',
    'price': 215,
    'imageUrl': '/rolls/roll-tayol.png'
  }, {
    'id': 7,
    'title': 'Ролл батакон',
    'weight': 220,
    'description': 'Огурец, сыр творожный, бекон',
    'price': 230,
    'imageUrl': '/rolls/roll-batacon.png'
  }, {
    'id': 8,
    'title': 'Ролл калипсо',
    'weight': 195,
    'description': 'Сыр творожный, огурец, мидии, стружка тунца',
    'price': 185,
    'imageUrl': '/rolls/roll-calypso.png'
  }, {
    'id': 9,
    'title': 'Ролл кавьяр',
    'weight': 240,
    'description': 'Сыр творожный, помидор, огурец, мидии, майонез',
    'price': 200,
    'imageUrl': '/rolls/roll-kavyar.png'
  }, {
    'id': 10,
    'title': 'Ролл чипу',
    'weight': 190,
    'description': 'Сыр творожный, лосось, томаго, стружка тунца',
    'price': 180,
    'imageUrl': '/rolls/roll-chipu.png'
  }, {
    'id': 11,
    'title': 'Ролл катана',
    'weight': 240,
    'description': 'Сыр творожный, краб, огурец, черный кунжут, лососевый тар-тар, спайси соус',
    'price': 250,
    'imageUrl': '/rolls/roll-katana.png'
  }, {
    'id': 12,
    'title': 'Ролл мишику',
    'weight': 210,
    'description': 'Угорь, краб, огурец, масаго, сыр творожный,  сырный соус, картофельные чипсы',
    'price': 260,
    'imageUrl': '/rolls/roll-mishiku.png'
  }
])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = event => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce(event => {
  filters.searchQuery = event.target.value
}, 500)

const changeBgButton = (index) => {
  return selectedMenuIndex.value === index ? ['bg-[#312525]', 'text-white'] : 'bg-white'
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://cacfd7a19d305bf4.mokky.dev/items', {
      params
    })
    items.value = data
  } catch (e) {
    console.log(e)
    items.value = items2.value
  }
}

onMounted(async () => {
  await fetchItems()
})

watch(filters, fetchItems)
</script>

<template>
  <div
    class="uppercase text-5xl 2xl:text-6xl text-center before:content-['\00B7'] after:content-['\00B7'] pt-12 pb-6 2xl:pt-16 2xl:pb-12">
    Меню
  </div>
  <ul
    class="text-2xl 2xl:text-3xl uppercase justify-around text-[#B3A296] cursor-pointer [&>li]:rounded-full [&>li]:p-4 flex flex-wrap">
    <li :class="changeBgButton(1)" @click="selectedMenuIndex = 1">Роллы</li>
    <li :class="changeBgButton(2)" @click="selectedMenuIndex = 2">Суши и гунканы</li>
    <li :class="changeBgButton(3)" @click="selectedMenuIndex = 3">Сеты</li>
    <li :class="changeBgButton(4)" @click="selectedMenuIndex = 4">Лапша и рис</li>
    <li :class="changeBgButton(5)" @click="selectedMenuIndex = 5">Салаты</li>
    <li :class="changeBgButton(6)" @click="selectedMenuIndex = 6">Горячие блюда</li>
  </ul>
  <div class="flex gap-4 flex-col lg:flex-row lg:-translate-x-2 justify-center p-9">
    <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none text-xl">
      <option value="title">По названию</option>
      <option value="price">По цене (сначала дешевле)</option>
      <option value="-price">По цене (сначала дороже)</option>
    </select>

    <div class="relative">
      <img class="absolute left-4 top-3" src="/search.svg" alt="Search">
      <input @input="onChangeSearchInput"
             class="border rounded-md py-2 pl-11 pr-6 outline-none focus:border-gray-400 placeholder:text-xl text-xl w-full"
             type="text" placeholder="Поиск"
      >
    </div>
  </div>
  <div v-if="selectedMenuIndex === 1" class="grid gap-5 p-6"
       style="grid-template-columns: repeat(auto-fill, minmax(300px, 1fr))" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :weight="item.weight"
      :description="item.description"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :item="item"
      :hideIsNotFavorite="false"
    />
  </div>
</template>