<script setup>
  import { ref, computed, onMounted } from 'vue';
  import { useToast } from 'vue-toastification';
  import axios from 'axios'; 
  import jsPDF from 'jspdf';
  import Loader from './Loader.vue'; 
  
  const beverages = ref([]);
  const tab = ref([]);
  const splitCount = ref(0);
  const roundNumber = ref(1); 
  const loading = ref(true); 
  const toast = useToast();

  const apiBaseUrl = import.meta.env.VITE_API_BASE_URL;
  
  const fetchBeverages = async () => {
  try {
    // Use the environment variable for the base API URL
    const response = await axios.get(`${apiBaseUrl}/beverages`);
    beverages.value = response.data; // Update the beverages state
  } catch (error) {
    console.error('Error fetching beverages:', error);
  } finally {
    loading.value = false; // Ensure loading state is updated
  }
};

const fetchFriends = async () => {
  try {
    // Use the environment variable for the base API URL
    const response = await axios.get(`${apiBaseUrl}/friends`);
    splitCount.value = response.data.numberOfFriends; // Update the splitCount state
  } catch (error) {
    console.error('Error fetching friends:', error);
  }
};
  
  onMounted(() => {
    fetchBeverages();
    fetchFriends();
  });
  
  const addToTab = async () => {
    beverages.value.forEach(beverage => {
      if (beverage.quantity > 0) {
        const existingDrink = tab.value.find(drink => drink.name === beverage.name);
        const totalPrice = beverage.price * beverage.quantity;
  
        if (existingDrink) {
          existingDrink.totalQuantity += beverage.quantity;
          existingDrink.totalPrice += totalPrice; 
        } else {
          tab.value.push({ name: beverage.name, totalQuantity: beverage.quantity, totalPrice });
        }
  
        beverage.quantity = 0; 
      }
    });
  
    if (tab.value.length > 0) {
      await submitOrder();
      roundNumber.value++; 
    }
  };
  
  const submitOrder = async () => {
  // Calculate the total price of the tab
  const totalPrice = tab.value.reduce((sum, drink) => sum + drink.totalPrice, 0);

  // Prepare the payload for the tab
  const payload = {
    splitCount: splitCount.value,
    totalPrice: totalPrice,
    date: new Date().toISOString(),
  };

  try {

    await axios.post(`${apiBaseUrl}/tabs/`, payload);

    
    await axios.post(`${apiBaseUrl}/orders`, {
      roundNumber: roundNumber.value,
      orderDetails: tab.value,
    });
  } catch (error) {
    console.error('Error submitting order:', error);
  }
};
  
  const total = computed(() => {
    return tab.value.reduce((sum, drink) => sum + drink.totalPrice, 0);
  });
  
  const exportToCSV = () => {
    const csvContent = "data:text/csv;charset=utf-8," 
      + tab.value.map(drink => `${drink.name},${drink.totalQuantity},${drink.totalPrice.toFixed(2)}`).join("\n")
      + `\nTotal,,${total.value.toFixed(2)}`
      + `\nPrice per person,,${splitCount.value > 0 ? (total.value / splitCount.value).toFixed(2) : total.value.toFixed(2)}`; 
  
    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", "tab_report.csv");
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    toast.success("CSV Exported");
  };
  
  const exportToPDF = () => {
    const doc = new jsPDF();
    doc.text("Your Tab Report", 20, 20);
    
    tab.value.forEach((drink, index) => {
      doc.text(`${drink.name} (x${drink.totalQuantity}) - R ${drink.totalPrice.toFixed(2)}`, 20, 30 + (10 * index));
    });
    
    doc.text(`Total: R ${total.value.toFixed(2)}`, 20, 30 + (10 * tab.value.length));
    
    if (splitCount.value > 0) {
      doc.text(`Price per person: R ${(total.value / splitCount.value).toFixed(2)}`, 20, 30 + (10 * (tab.value.length + 1)));
    } else {
      doc.text(`Price per person: R ${(total.value.toFixed(2))}`, 20, 30 + (10 * (tab.value.length + 1)));
    }
  
    doc.text(`Current Round: ${roundNumber.value}`, 20, 30 + (10 * (tab.value.length + 2))); 
  
    doc.save("tab_report.pdf");
    toast.success("PDF Exported");
  };

  const deleteAllOrderItems = async () => {
  try {

    // Fetch all order items
    const response = await axios.get(`${apiBaseUrl}/orders/`);
    const items = response.data;

    // Delete all order items
    const deletePromises = items.map(item => {
      return axios.delete(`${apiBaseUrl}/orders/${item.id}`);
    });

    await Promise.all(deletePromises);
    console.log('All order items deleted successfully.');
  } catch (error) {
    console.error('Error deleting order items:', error);
  }
};

const deleteAllTabItems = async () => {
  try {

    // Fetch all tab items
    const response = await axios.get(`${apiBaseUrl}/tabs/`);
    const items = response.data;

    // Delete all tab items
    const deletePromises = items.map(item => {
      return axios.delete(`${apiBaseUrl}/tabs/${item.id}`);
    });

    await Promise.all(deletePromises);
    console.log('All tab items deleted successfully.');
  } catch (error) {
    console.error('Error deleting tab items:', error);
  }
};

const payTab = async () => {
  loading.value = true;

  try {

    // Delete all order and tab items
    await deleteAllOrderItems();
    await deleteAllTabItems();

    // Reset music preferences
    await axios.put(`${apiBaseUrl}/music/`, { playMusic: false, selectedGenre: '' });

    // Reset friends preferences
    await axios.put(`${apiBaseUrl}/friends/`, { withFriends: false, numberOfFriends: 0 });

    // Reset beverages, tab, split count, and round number
    beverages.value.forEach(beverage => (beverage.quantity = 0));
    tab.value = [];
    splitCount.value = 0;
    roundNumber.value = 1;

    // Show success message
    toast.success('Tab Paid Up!');
  } catch (error) {
    console.error('Error resetting data:', error);
    toast.error('Error Paying Tab');
  } finally {
    loading.value = false;
  }
};
  </script>

<template>
    <div class="dashboard">
      <h1 class="text-2xl font-bold mb-4">Open Bar Tab</h1>
      
      <div v-if="loading" class="loader-container">
        <Loader />
      </div>
  
      <div v-else>
        <div class="grid grid-cols-2 gap-4">
          <div class="order-section">
            <div class="beverages mb-6">
              <h2 class="text-xl mb-2">Available Beverages</h2>
              <div v-for="(beverage, index) in beverages" :key="index" class="beverage-item mb-2">
                <span>{{ beverage.name }} - R {{ beverage.price.toFixed(2) }}</span>
                <input
                  v-model.number="beverage.quantity"
                  type="number"
                  min="0"
                  placeholder="Quantity"
                  class="border p-2 rounded ml-2"
                />
              </div>
            </div>
  
            <button @click="addToTab" class="bg-blue-500 text-white p-2 rounded mb-4">
              Submit Order
            </button>
  
            <div class="tab-section mb-6">
              <h2 class="text-xl mb-2">Your Tab</h2>
              <ul>
                <li v-for="(drink, index) in tab" :key="index">
                  {{ drink.name }} (x{{ drink.totalQuantity }}) - R {{ drink.totalPrice.toFixed(2) }}
                </li>
              </ul>
            </div>
          </div>
  
          <div class="summary-section">
            <div class="split-bill mb-4">
              <h2 class="text-xl mb-2">Split Bill</h2>
              <input
                v-model.number="splitCount"
                type="number"
                placeholder="Number of People"
                class="border p-2 rounded"
                disabled
              />
            </div>
  
            <p class="font-bold">Total: R {{ total.toFixed(2) }}</p>
            <p v-if="splitCount > 0" class="font-bold">
              Price per person: R {{ (total / splitCount).toFixed(2) }}
            </p>
  
            <div class="round-info mb-4">
              <h2 class="text-xl mb-2">Current Round: {{ roundNumber }}</h2>
            </div>
  
            <div class="export-section mt-4">
              <h2 class="text-xl mb-2">Export Options</h2>
              <button @click="exportToCSV" class="bg-green-500 text-white p-2 rounded mr-2">
                Export to CSV
              </button>
              <button @click="exportToPDF" class="bg-red-500 text-white p-2 rounded">
                Export to PDF
              </button>
            </div>
  
            <button @click="payTab" class="bg-red-600 text-white p-2 rounded mt-4">
              Pay Tab
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <style scoped>
  .dashboard {
    padding: 20px;
    max-width: 800px; 
    margin: 40px auto;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  .beverage-item {
    display: flex;
    align-items: center;
  }
  .grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  .loader-container {
    height: 100px; 
  }
  </style>
  