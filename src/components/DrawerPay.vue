<script setup>
import { inject, reactive, ref } from 'vue'
import axios from 'axios'

const props = defineProps({
  totalPrice: Number
})

const { cart } = inject('cart')
const emit = defineEmits(['closeDrawer'])
const deliveryValue = ref('delivery')
const name = ref('Д')
const phone = ref('+70000000000')
const lastCorrectPhone = ref('')
const message = ref('')
const isErrorEmptyInput = reactive({
  name: false,
  phone: false
})
const orderId = ref(null)
const testOrder = ref([])
const isOrderCreating = ref(false)
const onChangePhoneInput = (event) => {
  isErrorEmptyInput.phone = false
  if (isNaN(event.target.value)) {
    phone.value = lastCorrectPhone.value.trim()
  } else {
    lastCorrectPhone.value = event.target.value
  }
}
const createOrder = () => {
  if (name.value.trim().length === 0) {
    isErrorEmptyInput.name = true
    name.value = ''
  }
  if (phone.value.trim().length === 0) {
    isErrorEmptyInput.phone = true
    phone.value = ''
  }

  if (name.value.trim().length > 0 && phone.value.trim().length > 0) {
    console.log('Заказ создан!')
    console.log(props.totalPrice)
    testOrder.value.push({
      items: cart.value,
      deliveryValue: deliveryValue.value,
      name: name.value,
      phone: phone.value,
      message: message.value,
      totalPrice: props.totalPrice
    })
    console.log(JSON.stringify(testOrder.value))
    createOrderRequest()
  }
}
const createOrderRequest = async () => {
  try {
    isOrderCreating.value = true
    const { data } = await axios.post('https://cacfd7a19d305bf4.mokky.dev/orders', {
      items: cart.value,
      deliveryValue: deliveryValue.value,
      name: name.value,
      phone: phone.value,
      message: message.value,
      totalPrice: props.totalPrice
    })
    cart.value = []
    orderId.value = data.id
  } catch (e) {
    console.log(e)
  } finally {
    isOrderCreating.value = false
  }
}

</script>

<template>
  <div class="bg-[#EBE1D7] w-[400px] h-full fixed right-0 top-0 z-20 pt-10 pl-5 pr-5 overflow-y-auto">
    <img @click="() => emit('closeDrawer')"
         src="/close.svg" alt="Close"
         class="h-12 cursor-pointer absolute top-9 right-4 transition hover:scale-110 stroke-slate-400">

    <div class=" text-3xl text-center before:content-['\00B7'] after:content-['\00B7'] pb-7">
      Оформление заказа
    </div>

    <div v-if="!orderId" class="font-['Ubuntu']" :class="isOrderCreating ? 'opacity-50' : ''">
      <div class="p-4 pl-8 mt-3 bg-white rounded-xl shadow-md font-['Ubuntu'] text-xl">
        <div class="flex">
          <div
            class="bg-[#D34A44] -translate-x-3 w-8 h-8 text-white flex items-center justify-center rounded-full">
            1
          </div>
          <p>Способ доставки</p>
        </div>
        <div class="pt-4 pl-8">
          <div class="flex gap-4 cursor-pointer items-center" @click="() => isOrderCreating ? '' : deliveryValue = 'delivery'">
            <div :class="deliveryValue === 'delivery' ? 'bg-[#0ACF83]' : 'bg-white'"
                 class="w-8 h-8 border text-white rounded-full items-center justify-center flex">
              {{ deliveryValue === 'delivery' ? '✓' : '' }}
            </div>
            <img src="/delivery.svg" alt="Delivery">
            <div class="text-lg [&>p:nth-child(2)]:opacity-50">
              <p>Курьер</p>
              <!--            <div class="text-xs text-red-500">Временно недоступно</div>-->
            </div>
          </div>
          <div class="flex gap-4 pt-3 cursor-pointer" @click="() => isOrderCreating ? '' : deliveryValue = 'pickup'">
            <div :class="deliveryValue === 'pickup' ? 'bg-[#0ACF83]' : 'bg-white'"
                 class="w-8 h-8 border text-white rounded-full items-center justify-center flex">
              {{ deliveryValue === 'pickup' ? '✓' : '' }}
            </div>
            <img src="/shopping-bag.svg" alt="Delivery">
            <div class="text-lg">Самовывоз</div>
          </div>
        </div>
      </div>

      <div class="p-4 pl-8 mt-3 bg-white rounded-xl shadow-md font-['Ubuntu'] text-xl">
        <div class="flex">
          <div
            class="bg-[#D34A44] -translate-x-3 w-8 h-8 text-white flex items-center justify-center rounded-full">
            2
          </div>
          <p>Данные для доставки</p>
        </div>
        <div class="pt-4 pl-8">
          <img src="/icon-user.svg" alt="User" class="absolute pt-1">
          <input v-model="name" placeholder="Имя" :disabled="isOrderCreating"
                 class="pl-8 w-full border-b outline-none focus:border-gray-400 placeholder:text-base"
                 :class="isErrorEmptyInput.name ? 'border-red-500' : ''"
                 @input="isErrorEmptyInput.name = false; name = name.replace(/\s+/g, ' ')"
          />
          <p class="text-xs text-red-500" v-if="isErrorEmptyInput.name">Это обязательное поле</p>
        </div>
        <div class="pt-8 pl-8">
          <img src="/icon-phone.svg" alt="Phone" class="absolute pt-1">
          <input v-model="phone" placeholder="Телефон" :disabled="isOrderCreating"
                 class="pl-8 w-full border-b outline-none focus:border-gray-400 placeholder:text-base"
                 :class="isErrorEmptyInput.phone ? 'border-red-500' : ''"
                 @input="onChangePhoneInput"
          />
          <p class="text-xs text-red-500" v-if="isErrorEmptyInput.phone">Это обязательное поле</p>
        </div>
        <div class="pt-8 pl-8 pb-4">
          <img src="/icon-additional-information.svg" alt="Message" class="absolute pt-1">
          <input v-model="message" placeholder="Дополнительная информация" :disabled="isOrderCreating"
                 class="pl-8 w-full border-b outline-none focus:border-gray-400 placeholder:text-base" />
        </div>
      </div>

      <div class="p-4 pl-8 mt-8 bg-white rounded-xl shadow-md font-['Ubuntu'] text-lg">
        <div class="flex gap-2">
          Итого к оплате:
          <div class="border-b border-dashed flex-1"></div>
          <b>{{ totalPrice.toLocaleString() }} руб.</b>
        </div>
      </div>

      <button
        :disabled="isOrderCreating"
        @click="createOrder"
        class="mt-4 bg-[#312525] text-xl disabled:bg-slate-400 w-full rounded-xl py-3 text-white transition hover:bg-[#4e3b3b] active:bg-[#664d4d] cursor-pointer">
        Купить и оплатить
      </button>
    </div>
     <div v-else class="text-center justify-center h-full flex flex-col -mt-24 text-3xl">
      <p>Заказ оформлен!</p>
      <p v-if="deliveryValue === 'delivery'" class="text-xl pt-6 text-gray-500">Ваш заказ скоро будет передан службе курьерской доставки</p>
       <p v-else class="text-xl pt-6 text-gray-500">Скоро вы сможете забрать ваш заказ</p>
    </div>
  </div>
</template>