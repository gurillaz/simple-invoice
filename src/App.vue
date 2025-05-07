
<template>
  <div class="grid grid-cols-11">

    
    <div class="hidden lg:block col-span-6 h-screen overflow-auto no_print">
      <InvoiceForm
        v-model:formData="invoiceData"
        @generate-pdf="openPrintView"
      />
    </div>
    <div
      ref="scrollableDiv"
      class="hidden lg:block col-span-5 bg-gradient-to-tr bg-amber-300 clear_view overflow-auto h-screen py-2 px-6"
    >
      <InvoicePreview :invoiceData="invoiceData" />
    </div>
    
    <div class="lg:hidden col-span-11 p-8 text-center no_print">
      <h2 class="text-xl font-bold mb-4">Eingeschränkte Funktionalität</h2>
      <p class="mb-4">Dieses Rechnungstool ist für die Nutzung auf Desktop-Computern optimiert.</p>
      <p>Bitte wechseln Sie zu einem Gerät mit größerem Bildschirm, um alle Funktionen nutzen zu können.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from "vue";
import InvoiceForm from "./components/InvoiceForm.vue";
import InvoicePreview from "./components/InvoicePreview.vue";

const STORAGE_KEY = 'invoiceAppData';

// Load only the specific fields we want to persist
const loadSavedData = () => {
  const savedData = localStorage.getItem(STORAGE_KEY);
  const defaultData = {
    invoiceNumber: "10500",
    from: {
      name: "",
      address: "",
      email: "",
      phone: "",
    },
    notes: "Wir freuen uns, mit Ihnen zusammenarbeiten zu dürfen.",
  };

  if (savedData) {
    try {
      const parsedData = JSON.parse(savedData);
      // Increment the invoice number when loading
      if (parsedData.invoiceNumber) {
        parsedData.invoiceNumber = String(Number(parsedData.invoiceNumber) + 1);
      }
      return {
        ...defaultData,
        ...parsedData
      };
    } catch (e) {
      return defaultData;
    }
  }
  return defaultData;
};

// Initialize with full structure but only persist specific fields
const invoiceData = ref({
  invoiceNumber: loadSavedData().invoiceNumber,
  date: new Date().toISOString().split("T")[0],
  dueDate: new Date(Date.now() + 31 * 24 * 60 * 60 * 1000)
    .toISOString()
    .split("T")[0],
  from: loadSavedData().from,
  to: {
    name: "",
    address: "",
    email: "",
    phone: "",
  },
  items: [],
  taxRate: 0,
  notes: loadSavedData().notes,
});

// Only save the specific fields we want to persist
watch(
  () => ({
    invoiceNumber: invoiceData.value.invoiceNumber,
    from: invoiceData.value.from,
    notes: invoiceData.value.notes
  }),
  (newValue) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newValue));
  },
  { deep: true }
);

const openPrintView = () => {
  try {
    const originalTitle = document.title;
    document.title = "Rechnung_" + invoiceData.value.to.name + "_" + invoiceData.value.invoiceNumber;
    window.print();
    setTimeout(() => {
      document.title = originalTitle;
    }, 1000);
  } catch (error) {
    console.error("Fehler beim Öffnen der Druckansicht:", error);
  }
};
</script>
<style scoped></style>