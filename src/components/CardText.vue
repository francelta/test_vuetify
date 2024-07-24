<template>
    <v-card>
      <v-card-title>cliente número {{ clientNumber }} / fecha: {{ currentDate }}</v-card-title>
      <v-card-text>
        <div v-if="orders.length">
          <div v-for="(order, index) in orders" :key="index" class="mb-4">
            <h3>{{ order.name }} ({{ order.quantity }})</h3>
            <ul class="no-bullets">
              <li v-for="(ingredient, iIndex) in order.ingredients" :key="iIndex">
                {{ ingredient }}
              </li>
            </ul>
            <p>Total: €{{ order.totalPrice.toFixed(2) }}</p>
          </div>
          <div class="total-section">
            <h3>Total del Pedido: €{{ totalOrderPrice.toFixed(2) }}</h3>
            <v-btn class="pr-4" color="green" @click="payOrder">Pagar</v-btn>
            <v-btn color="red" @click="cancelOrder">Cancelar</v-btn>
            <v-btn color="orange" @click="resetOrder">Reiniciar Pedido</v-btn>
          </div>
        </div>
  
        <div v-else>
          <p>No hay pedidos aún.</p>
        </div>
      </v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    name: 'CardText',
    data() {
      return {
        orders: [],
        clientNumber: '',
        currentDate: ''
      };
    },
    computed: {
      totalOrderPrice() {
        return this.orders.reduce((total, order) => total + order.totalPrice, 0);
      }
    },
    created() {
      this.loadOrders();
      this.generateClientNumber();
      this.setCurrentDate();
    },
    methods: {
      loadOrders() {
        const orders = JSON.parse(localStorage.getItem('orders')) || [];
        this.orders = orders;
      },
      payOrder() {
        alert('Orden pagada con éxito!');
        this.resetOrder();
      },
      cancelOrder() {
        alert('Orden cancelada.');
        this.resetOrder();
      },
      resetOrder() {
        this.orders = [];
        localStorage.removeItem('orders');
        this.$emit('order-updated');
      },
      generateClientNumber() {
        this.clientNumber = Math.floor(10000 + Math.random() * 90000).toString();
      },
      setCurrentDate() {
        const today = new Date();
        const dd = String(today.getDate()).padStart(2, '0');
        const mm = String(today.getMonth() + 1).padStart(2, '0');
        const yyyy = today.getFullYear();
        this.currentDate = dd + '/' + mm + '/' + yyyy;
      }
    }
  };
  </script>
  
  <style scoped>
  .mb-4 {
    margin-bottom: 16px;
  }
  
  .no-bullets {
    list-style-type: none;
    padding-left: 0;
  }
  
  .total-section {
    text-align: center;
    margin-top: 24px;
    
  }
  
  .total-section h3 {
    margin-bottom: 16px;
  }
  
  .total-section v-btn {
    margin: 8px;
    
  }
  </style>
  
