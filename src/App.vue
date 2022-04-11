<template>
  <div class="w-screen h-screen bg-gray-800 flex flex-row text-white items-start justify-center">
      <app-sidebar  :editInvoices="editInvoices"  :invoiceList="invoiceList"/>
      <app-invoice-content :saveInvoice="saveInvoice" :activeInvoices="state.activeInvoices"/>
  </div>
</template>
<script setup>
import {ref, reactive} from 'vue'
import appSidebar from './components/appSidebar.vue'
import appInvoiceContent from './components/appInvoiceContent.vue'


const invoiceList = ref([
  {
    id: new Date().getTime(),
    contact: {
      contact_name: 'Kemal',
      email: 'kemal@gmail.com',
      city: 'izmir',
      country: 'Türkiye',
      zipcode: '35523',
    },
    items: [
      {
        id: new Date().getTime(),
        name: 'demo',
        quantity: 1,
        unit_price: 10.0,
        total_price: 10.0,
      }
    ],
  }
])
const saveInvoice = (invoice) => {
    invoiceList.value.push(invoice)
}

const state =  reactive({activeInvoices: null})
const editInvoices = ((invoice) => {
  state.activeInvoices = invoice
})

</script>