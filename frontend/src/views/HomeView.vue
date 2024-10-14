<template>
  <div>
    <h1>Products Lis for my website</h1>
    <div class="product-grid">
      <div class="product-card" v-for="item in products" :key="item.id">
        <img :src="`http://localhost:1337${item.image.url}`" alt="Vivi's image" class="product-image" />
        <h2 class="product-name">{{ item.name }}</h2>
        <p class="product-description">{{ item.description }}</p>
        <p class="product-price">Price: <span class="price">{{ item.price }} Baht</span></p>
        <p v-if="item.isOnSale" class="on-sale">Status: On Sale</p>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'ProductList',
  setup() {
    const products = ref([]);
    const defaultImage = 'https://via.placeholder.com/250'; // Placeholder image URL

    const fetchData = () => {
      // Replace the URL with your API endpoint
      fetch('http://localhost:1337/api/products?populate=*') // Make sure this URL is correct
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          products.value = data.data; // Assuming your response structure is as shown in your initial JSON
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    };

    onMounted(fetchData);

    return { products };
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f8f9fa; /* Light gray background */
  margin: 0;
  padding: 20px;
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); /* Responsive columns */
  gap: 20px; /* Space between items */
}

.product-card {
  background: white; /* White background for cards */
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 15px;
  transition: transform 0.2s; /* Smooth hover effect */
}

.product-card:hover {
  transform: translateY(-5px); /* Lift effect on hover */
}

.product-image {
  width: 100%; /* Full width */
  height: auto; /* Maintain aspect ratio */
  border-radius: 8px; /* Rounded corners for images */
  margin-bottom: 10px; /* Space below the image */
}

.product-name {
  font-size: 1.5em; /* Larger font size for product name */
  color: #333; /* Dark text color */
  margin: 0 0 10px; /* Margin below the name */
}

.product-description {
  color: #666; /* Slightly lighter color for description */
}

.product-price {
  font-weight: bold; /* Bold for price */
  margin: 10px 0; /* Margin above and below price */
}

.price {
  color: #007bff; /* Blue color for price */
}

.on-sale {
  color: #ff4d4d; /* Red color for sale status */
  font-weight: bold; /* Bold for sale status */
}

h1 {
  text-align: center; /* Center align the header */
  color: #333; /* Dark text color */
  margin-bottom: 20px; /* Margin below the header */
}
</style>
