<script setup>
import Card from '@/components/Card.vue'
import { computed, onMounted, ref } from 'vue'
import axios from 'axios'

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
const favorites = ref()
const requestCompleted = ref(false)
const onlyFavoriteItems = computed(() => items.value.filter(item => favorites.value ? favorites.value.includes(item.id) : ''))
const fetchItems = async () => {
  try {
    const { data } = await axios.get('https://cacfd7a19d305bf4.mokky.dev/items')
    items.value = data
  } catch (e) {
    console.log(e)
    items.value = items2.value
  } finally {
    requestCompleted.value = true
  }
}

onMounted(() => {
  favorites.value = localStorage.getItem('favorites2')
  fetchItems()
})
</script>
<template>
  <div
    class="uppercase text-5xl 2xl:text-6xl text-center before:content-['\00B7'] after:content-['\00B7'] pt-12 pb-6 2xl:pt-16 2xl:pb-12">
    Закладки
  </div>
  <div class="grid gap-5 p-6"
       style="grid-template-columns: repeat(auto-fill, minmax(300px, 1fr))" v-auto-animate>
    <Card
      v-for="item in onlyFavoriteItems"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :weight="item.weight"
      :description="item.description"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :item="item"
      :hideIsNotFavorite="true"
    />
  </div>
  <div v-if="!onlyFavoriteItems.length && requestCompleted">
    <div class="h-96 flex flex-col justify-center text-center">
      <h2 class="text-4xl font-bold mb-8">Закладок нет</h2>
      <p class="text-xl">Добавляйте закладки, чтобы увидеть их здесь</p>
    </div>
  </div>
</template>
