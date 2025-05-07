<template>
  <div class="print_preview">
      <div class="grid grid-cols-2">
        <div>
          <p class="text-2xl font-bold">{{ invoiceData.from.name }}</p>
          <p>{{ invoiceData.from.address }}</p>
          <p>{{ invoiceData.from.email }}</p>
          <p>{{ invoiceData.from.phone }}</p>
        </div>
        <div class="text-right">
          <p class="text-2xl">RECHNUNG</p>
        </div>
      </div>
      <div class="grid grid-cols-6 mt-10">
        <div class="col-span-3">
          <p class="text-1xl font-bold">Rechnungsempfänger</p>
          <p>{{ invoiceData.to.name }}</p>
          <p>{{ invoiceData.to.address }}</p>
          <p>{{ invoiceData.to.email }}</p>
          <p>{{ invoiceData.to.phone }}</p>
        </div>
        <div class="col-span-3">
          <div class="grid grid-cols-12 gap-3">
            <div class="col-span-7 text-right font-bold">
              <p>Rechnungsnummer:</p>
              <p>Rechnungsdatum:</p>
              <p>Fälligkeitsdatum:</p>
            </div>
            <div class="col-span-5 text-right">
              <p>R-{{ invoiceData.invoiceNumber }}</p>
              <p>{{ formatDate(invoiceData.date) }}</p>
              <p>{{ formatDate(invoiceData.dueDate) }}</p>
            </div>
          </div>
        </div>
      </div>

      <div class="grid mt-10">
        <table class="table-auto">
          <thead class="bg-gray-700 font-bold text-white h-[50px]">
            <tr class="">
              <th class="pl-2 text-left">Pos.</th>
              <th class="pl-2 text-left">Leistung/Beschreibung</th>
              <th class="pl-2 text-center">Menge</th>
              <th class="pl-2 text-center">Einzelpreis</th>
              <th class="px-2 text-right">Gesamt</th>
            </tr>
          </thead>
          <tbody>
            <tr
              class="h-[50px] border-b border-b-gray-700"
              v-for="(item, index) in invoiceData.items"
              :key="index"
            >
              <td class="pl-2 text-center">{{ index+1 }}</td>
              <td class="pl-2 text-left text-wrap break-words">{{ item.description }}</td>
              <td class="pl-2 text-center">{{ item.quantity }}</td>
              <td class="pl-2 text-center">{{ formatCurrency(item.price) }}</td>
              <td class="pl-2 text-right">
                {{ formatCurrency(item.quantity * item.price) }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="grid grid-cols-1 mt-10">
        <div class="text-right">
  
   
          <p>
            Rechnungsbetrag:
            <span class="ml-10 font-bold">{{ formatCurrency(total) }}</span>
          </p>
        </div>
      </div>

      <div class="grid grid-cols-1 mt-60">
        <div class="">
          <p class="text-1xl font-bold text-gray-700">Bemerkungen:</p>
          <p>{{ invoiceData.notes }}</p>
        </div>
      </div>
    </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  invoiceData: {
    type: Object,
    required: true,
  },
});

const subtotal = computed(() => {
  return props.invoiceData.items.reduce((sum, item) => {
    return sum + item.quantity * item.price;
  }, 0);
});

const taxAmount = computed(() => {
  return subtotal.value * (props.invoiceData.taxRate / 100);
});

const total = computed(() => {
  return subtotal.value + taxAmount.value;
});

const formatCurrency = (value) => {
  return new Intl.NumberFormat("de-DE", {
    style: "currency",
    currency: "EUR",
  }).format(value);
};

const formatDate = (dateString) => {
  const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
  return new Date(dateString).toLocaleDateString('de-DE', options);
};
</script>

<style scoped>
.print_preview {
  padding: 15mm;
  min-height: 95vh; 
  margin: 20px auto;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  background: white;
}
</style>