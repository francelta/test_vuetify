<template>
    <v-container style="padding-top: 70px; padding-left: 70px;">
      <v-nav>
        <v-btn class="d-flex justify-end" to="/carrito" prepend-icon="mdi-cart">Comprar</v-btn>
      </v-nav>
  
      <v-row class="pt-3">
        <v-col :cols="hasOrders ? 7 : 12" class="d-flex flex-column align-center mb-12">
          <h1 class="centered-text">Menú del Sitio</h1>
          <v-row>
            <v-col v-for="(food, index) in foods" :key="index" cols="12">
              <CardPhoto :title="food.name" :subtitle="food.price" :img-src="food.imgSrc" action="Agregar al carrito" @order-updated="updateOrders" />
            </v-col>
          </v-row>
        </v-col>
        <v-col v-if="hasOrders" cols="5" class="d-flex flex-column align-center">
          <h1 class="centered-text">Pedido</h1>
          <CardText ref="cardText" @order-updated="updateOrders"></CardText>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import CardPhoto from '@/components/CardPhoto.vue';
  import CardText from '@/components/CardText.vue';
  
  export default {
    name: 'HomeView',
    components: {
      CardPhoto,
      CardText
    },
    data() {
      return {
        foods: [
          { name: 'Pizza Margherita', price: '€10', imgSrc: require('@/assets/pizza_margherita.png') },
          { name: 'Hamburguesa Clásica', price: '€8', imgSrc: require('@/assets/classic_hamburger.png') },
          { name: 'Ensalada César', price: '€7', imgSrc: require('@/assets/caesar_salad.png') },
          { name: 'Sushi Roll', price: '€12', imgSrc: require('@/assets/sushi_roll.png') },
          { name: 'Pasta Alfredo', price: '€11', imgSrc: require('@/assets/alfredo_pasta.png') },
          { name: 'Taco de Pollo', price: '€5', imgSrc: require('@/assets/chicken_taco.png') },
          { name: 'Helado de Vainilla', price: '€3', imgSrc: require('@/assets/vanilla_ice_cream.png') },
        ],
        orders: [] // Definir orders aquí
      };
    },
    computed: {
      hasOrders() {
        return this.orders && this.orders.length > 0; // Asegurarse de que orders esté definido
      }
    },
    methods: {
      updateOrders() {
        this.orders = JSON.parse(localStorage.getItem('orders')) || [];
      }
    },
    watch: {
      orders(newOrders) {
        if (this.$refs.cardText) {
          this.$refs.cardText.loadOrders();
        }
      }
    },
    mounted() {
      this.updateOrders();
      if (this.$refs.cardText) {
        this.$refs.cardText.loadOrders();
      }
    }
  };
  </script>
  
  <style scoped>
  .pt-3 {
    padding-top: 24px !important;
  }
  
  .mb-12 {
    margin-bottom: 48px !important;
  }
  
  .centered-text {
    text-align: center;
    width: 100%;
    padding-top: 2vh;
    padding-bottom: 7vh;
  }
  </style>
  