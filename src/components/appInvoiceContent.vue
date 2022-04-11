<template>
  <section class="bg-gray-900 w-1/3 mx-auto mt-10 p-2 px-5 rounded-md shadow-2xl">

    <contact-section :contact="state.contact" />

  <div class="mt-5">
    <heading title="Fatura Kalemleri" />
    <invoice-items :items="state.items" :AddInvoiceItem="AddInvoiceItem" :DeleteInvoiceItem="DeleteInvoiceItem" />

  </div>
  <!-- Summary -->
    <invoice-summary :items="state.items" />

  <hr class="bg-gradient-to-r h-[1px] border-none from-gray-700 mt-5" />
  <div class="actionbar text-right my-5">
    <button @click="onSubmit"  class="purple-button">Kaydet</button>
  </div>
  </section>
</template>
<script setup>
import InvoiceItems from './invoiceItems.vue'
import InvoiceSummary from "./invoiceSummary.vue";
import ContactSection from "./contactSection.vue";
import {reactive, provide, watch} from "vue";

const props = defineProps({
    saveInvoice: {
        type: Function,
        required: true
    },
  activeInvoices:[Object, null]

})

const state = reactive({
    contact: {
      contact_name: null,
      email: null,
      city: null,
      country: null,
      zipcode: null,
    },
    items: [],
})

const AddInvoiceItem= () => {
  state.items.push({
    id: new Date().getTime(),
    name: null,
    quantity: null,
    unit_price: 0.0,
    total_price: 0.0,
  })
}

const DeleteInvoiceItem = (invoiceItem) => {
  // itemlarda id 'ye göre parametre olarak gelen itema eşit olmayanları tekrar listeler, eşit olanı filtreler yani siler
 state.items = state.items.filter(item => item.id !== invoiceItem.id)
}
provide('DeleteInvoiceItem', DeleteInvoiceItem)

const onSubmit = () => {
  props.saveInvoice({...state, created_at: new Date(), id: new Date().getTime()})
  state.contact = {}
  state.items = []
}


watch(() =>props.activeInvoices, (activeInvoices) => {
  if (activeInvoices) {
    state.contact = {...activeInvoices.contact}
    state.items = [...activeInvoices.items]
  }
})


</script>