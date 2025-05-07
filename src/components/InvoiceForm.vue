<template>
  <div class="container pt-5 px-10">
    <h2 class="font-bold text-1xl w-full text-center">Rechnungserstellung</h2>
    <form class="mx-auto" @submit.prevent="$emit('generate-pdf')">
      <!-- Firmendaten -->
      <div class="mb-5">
        <div class="grid grid-cols-6 gap-2">
          <h2 class="col-span-6 font-medium text-1xl text-left">Firmendaten</h2>
          <div class="col-span-2">
            <label for="fromName" class="block mb-1 text-sm font-medium"
              >Firmenname</label
            >
            <input
              type="text"
              id="fromName"
              v-model="formData.from.name"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="Firmenname"
              required
            />
          </div>
          <div class="col-span-2">
            <label for="fromEmail" class="block mb-1 text-sm font-medium"
              >E-Mail</label
            >
            <input
              type="email"
              v-model="formData.from.email"
              id="fromEmail"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="info@firma.de"
            />
          </div>

          <div class="col-span-2">
            <label for="fromPhone" class="block mb-1 text-sm font-medium"
              >Telefon</label
            >
            <input
              type="text"
              v-model="formData.from.phone"
              id="fromPhone"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="089 1234567"
            />
          </div>
          <div class="col-span-6">
            <label for="fromAdress" class="block mb-1 text-sm font-medium"
              >Adresse</label
            >
            <input
              type="text"
              v-model="formData.from.address"
              name="fromAdress"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="Musterstraße 1, 80331 München"
              required
            />
          </div>
        </div>
      </div>

      <!-- Kundendaten -->
      <div class="mb-5">
        <div class="grid grid-cols-6 gap-4">
          <h2 class="col-span-6 font-medium text-1xl text-left">
            Rechnungsempfänger
          </h2>
          <div class="col-span-2">
            <label for="toName" class="block mb-1 text-sm font-medium"
              >Name</label
            >
            <input
              v-model="formData.to.name"
              type="text"
              id="toName"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="Max Mustermann"
              required
            />
          </div>
          <div class="col-span-4">
            <label for="toAdress" class="block mb-1 text-sm font-medium"
              >Adresse</label
            >
            <input
              type="text"
              v-model="formData.to.address"
              id="toAdress"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="Beispielstraße 12, 10115 Berlin"
              required
            />
          </div>

          <div class="col-span-3">
            <label for="toPhone" class="block mb-1 text-sm font-medium"
              >Telefon</label
            >
            <input
              type="text"
              id="toPhone"
              v-model="formData.to.phone"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="030 9876543"
            />
          </div>
          <div class="col-span-3">
            <label for="toEmail" class="block mb-1 text-sm font-medium"
              >E-Mail</label
            >
            <input
              id="toEmail"
              v-model="formData.to.email"
              type="email"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="info@kunde.de"
            />
          </div>
        </div>
      </div>

      <!-- Rechnungsdaten -->
      <div class="mb-5 mt-10">
        <div class="grid grid-cols-6 gap-4">
          <div class="col-span-2">
            <label for="invoiceNumber" class="block mb-1 text-sm font-medium"
              >Rechnungsnummer:</label
            >
            <input
              type="number"
              min="10500"
              v-model="formData.invoiceNumber"
              id="invoiceNumber"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="10500"
              required
            />
          </div>
          <div class="col-span-2">
            <label for="invoiceDate" class="block mb-1 text-sm font-medium"
              >Rechnungsdatum</label
            >
            <input
              id="invoiceDate"
              type="date"
              v-model="formData.date"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              required
            />
          </div>

          <div class="col-span-2">
            <label for="dueDate" class="block mb-1 text-sm font-medium"
              >Fälligkeitsdatum</label
            >
            <input
              v-model="formData.dueDate"
              id="dueDate"
              type="date"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
            />
          </div>
        </div>
      </div>

      <!-- Artikel -->
      <div class="mb-5 mt-5">
        <div class="grid grid-cols-12 gap-2">
          <h2 class="col-span-12 font-medium text-1xl text-left">Leistungen</h2>

          <div
            v-if="formData.items.length == 0"
            class="col-span-12 text-center mb-5"
          >
            <p class="text-center text-gray-400">
              Keine Artikel vorhanden. Bitte fügen Sie Leistungen hinzu.
            </p>
          </div>

          <div class="col-span-12 grid grid-cols-12" v-else>
            <div class="col-span-1"><p>Pos.</p></div>
            <div class="col-span-6"><p>Beschreibung</p></div>
            <div class="col-span-2"><p>Menge</p></div>
            <div class="col-span-2"><p>Einzelpreis (€)</p></div>
            <div class="col-span-1"><p></p></div>
          </div>

          <!-- Artikel in Rechnung -->
          <div
            class="col-span-12 grid grid-cols-12"
            v-for="(item, index) in formData.items"
          >
            <div class="col-span-1">
              <input
                type="text"
                :value="index + 1"
                class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
                readonly
              />
            </div>
            <div class="col-span-6">
              <input
                v-model="item.description"
                type="text"
                class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
                required
              />
            </div>
            <div class="col-span-2">
              <input
                v-model="item.quantity"
                type="number"
                min="1"
                required
                class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              />
            </div>
            <div class="col-span-2">
              <input
                v-model="item.price"
                type="number"
                min="0"
                step="0.01"
                required
                class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              />
            </div>

            <div class="col-span-1 flex items-end">
              <button
                type="button"
                @click="removeItem(index)"
                class="text-white bg-red-600 hover:bg-red-700 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-none text-sm px-5 w-full py-2 text-center"
              >
                -
              </button>
            </div>
          </div>
        </div>
        
        <!-- Neuer Artikel -->
        <div class="grid grid-cols-12 gap-0 mt-5">
          <div class="col-span-7"><p>Leistungsbeschreibung</p></div>
          <div class="col-span-2"><p>Menge</p></div>
          <div class="col-span-2"><p>Einzelpreis (€)</p></div>
          <div class="col-span-1"><p></p></div>

          <div class="col-span-7">
            <input
              v-model="newArticle.description"
              type="text"
              class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
            />
          </div>
          <div class="col-span-2">
            <input
              v-model="newArticle.quantity"
              type="number"
              step="1"
              class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
            />
          </div>
          <div class="col-span-2">
            <input
              type="number"
              v-model="newArticle.price"
              min="0"
              step="0.01"
              class="border border-gray-300 text-gray-900 text-sm rounded-none focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
            />
          </div>

          <div class="col-span-1 flex items-end">
            <button
              type="button"
              @click="addItem"
              class="text-white bg-green-600 hover:bg-green-700 focus:ring-4 focus:outline-none focus:ring-green-300 font-medium rounded-none text-sm px-5 w-full py-2 text-center"
            >
              <span class="font-black text-1xl">+</span>
            </button>
          </div>
        </div>
      </div>

      <!-- Anmerkungen -->
      <div class="mb-5">
        <div class="grid grid-cols-2 gap-4">
          <div class="col-span-2">
            <label for="notes" class="block mb-1 text-sm font-medium"
              >Bemerkungen</label
            >
            <input
              id="notes"
              type="text"
              v-model="formData.notes"
              class="border border-gray-300 text-gray-900 text-sm rounded-sm focus:ring-blue-500 focus:border-blue-500 block w-full p-2"
              placeholder="Vielen Dank für Ihren Auftrag."
            />
          </div>
        </div>
      </div>

      <!-- Submit Button -->
      <div class="mb-5">
        <div class="grid grid-cols-2">
          <button
            type="submit"
            class="col-span-2 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-sm text-sm px-5 py-2.5 text-center"
          >
            PDF erstellen
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
const props = defineProps({
  formData: {
    type: Object,
    required: true,
  },
});
let newArticle = { description: "", quantity: 0, price: 0 };

const emit = defineEmits(["generate-pdf", "update:formData"]);

const addItem = () => {
  const newItems = [
    ...props.formData.items,
    { ...newArticle }, // Create a new copy
  ];
  emit("update:formData", { ...props.formData, items: newItems });
  // Reset the form
  newArticle.description = "";
  newArticle.quantity = 0;
  newArticle.price = 0;
};

const removeItem = (index) => {
  if (props.formData.items.length <= 0) return;
  const newItems = [...props.formData.items];
  newItems.splice(index, 1);
  emit("update:formData", { ...props.formData, items: newItems });
};
</script>
