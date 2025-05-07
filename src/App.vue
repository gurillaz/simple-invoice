<template>
  <div class="grid grid-cols-11">
    <div class="col-span-6 h-screen overflow-auto no_print">
      <InvoiceForm
        v-model:formData="invoiceData"
        @generate-pdf="openPrintView"
      />
    </div>
    <div
      ref="scrollableDiv"
      class="col-span-5 bg-gradient-to-tr bg-amber-300 clear_view overflow-auto h-screen py-2 px-6"
    >
      <InvoicePreview :invoiceData="invoiceData" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import InvoiceForm from "./components/InvoiceForm.vue";
import InvoicePreview from "./components/InvoicePreview.vue";

const invoiceData = ref({
  invoiceNumber: "10500",
  date: new Date().toISOString().split("T")[0],
  dueDate: new Date(Date.now() + 31 * 24 * 60 * 60 * 1000)
    .toISOString()
    .split("T")[0],
  from: {
    name: "",
    address: "",
    email: "",
    phone: "",
  },
  to: {
    name: "",
    address: "",
    email: "",
    phone: "",
  },
  items: [],
  taxRate: 0,
  notes: "Wir freuen uns, mit Ihnen zusammenarbeiten zu dürfen.",
});

const openPrintView = () => {
  try {
    // Store original title
    const originalTitle = document.title;

    // Set new title for printing
    document.title =
      "Rechnung_" + invoiceData.value.to.name + "_" + invoiceData.value.invoiceNumber;

    // Print and restore original title
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