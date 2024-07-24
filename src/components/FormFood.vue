<template>
  <v-dialog v-model="internalDialog" max-width="600">
    <v-card>
      <v-card-title class="headline">Selecciona los ingredientes extras</v-card-title>
      <v-card-text>
        <v-form>
          <v-row>
            <v-col cols="12" class="d-flex align-center">
              <v-btn icon @click="decrementQuantity">
                <v-icon>mdi-minus</v-icon>
              </v-btn>
              <span class="mx-2">{{ quantity }}</span>
              <v-btn icon @click="incrementQuantity">
                <v-icon>mdi-plus</v-icon>
              </v-btn>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="internalName"
                label="Nombre del plato"
                readonly
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="internalBasePrice"
                label="Precio base"
                prefix="€"
                readonly
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-checkbox
                v-for="(ingredient, index) in ingredients"
                :key="index"
                :label="ingredient.name"
                :value="ingredient.name"
                @change="updateTotalPrice(ingredient)"
              ></v-checkbox>
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="totalPrice"
                label="Precio total"
                prefix="€"
                readonly
              ></v-text-field>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="red" text @click="cancel">Cancelar</v-btn>
        <v-btn color="green" text @click="finalize">Añadir</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'FormFood',
  props: {
    dialog: {
      type: Boolean,
      required: true
    },
    name: {
      type: String,
      required: true
    },
    basePrice: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      internalDialog: this.dialog,
      internalName: this.name,
      internalBasePrice: this.basePrice,
      quantity: 1,
      ingredients: [
        { name: 'Queso extra', price: 1.5 },
        { name: 'Bacon', price: 2 },
        { name: 'Champiñones', price: 1 },
        { name: 'Cebolla caramelizada', price: 1.2 },
        { name: 'Aguacate', price: 2.5 }
      ],
      selectedIngredients: [],
      totalPrice: this.basePrice
    }
  },
  watch: {
    dialog(val) {
      this.internalDialog = val;
    },
    internalDialog(val) {
      this.$emit('update:dialog', val);
    },
    name(val) {
      this.internalName = val;
    },
    basePrice(val) {
      this.internalBasePrice = val;
    }
  },
  methods: {
    incrementQuantity() {
      this.quantity++;
      this.updateTotalPrice();
    },
    decrementQuantity() {
      if (this.quantity > 1) {
        this.quantity--;
        this.updateTotalPrice();
      }
    },
    updateTotalPrice(ingredient = null) {
      if (ingredient) {
        const index = this.selectedIngredients.indexOf(ingredient.name);
        if (index === -1) {
          this.selectedIngredients.push(ingredient.name);
        } else {
          this.selectedIngredients.splice(index, 1);
        }
      }

      let extraCost = 0;
      this.selectedIngredients.forEach(ingredientName => {
        const ingredient = this.ingredients.find(i => i.name === ingredientName);
        if (ingredient) {
          extraCost += ingredient.price;
        }
      });
      this.totalPrice = (this.internalBasePrice + extraCost) * this.quantity;
    },
    finalize() {
      const order = {
        name: this.internalName,
        quantity: this.quantity,
        ingredients: this.selectedIngredients,
        totalPrice: this.totalPrice
      };
      let orders = JSON.parse(localStorage.getItem('orders')) || [];
      orders.push(order);
      localStorage.setItem('orders', JSON.stringify(orders));
      this.$emit('finalize'); // Emitimos finalize en lugar de close
    },
    cancel() {
      this.$emit('close');
    },
    resetForm() {
      this.quantity = 1;
      this.selectedIngredients = [];
      this.totalPrice = this.internalBasePrice;
    }
  }
}
</script>

<style scoped>
.mx-2 {
  margin-left: 8px;
  margin-right: 8px;
}
</style>
