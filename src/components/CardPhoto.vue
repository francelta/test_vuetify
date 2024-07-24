<template>
    <v-card variant="tonal">
      <v-row>
        <v-col cols="2" class="d-flex align-center justify-center ml-5" style="max-width: 13%;">
        <div class="image-container">
          <v-img :src="imgSrc" class="rounded-circle" cover @click="dialog = true"></v-img>
        </div>
      </v-col>
  
        <v-col cols="4">
          <v-col cols="12" class="py-5">
            <v-btn block dark class="pa-0" elevation="0" @click="openInfo = true">
              <h4 class="white--text m-1 justify-left">{{ title }}</h4>
            </v-btn>
            <v-card-subtitle>{{ subtitle }}</v-card-subtitle>
          </v-col>
        </v-col>
  
        <v-col cols="2">
          <v-card-text>{{ text }}</v-card-text>
        </v-col>
  
        <v-col cols="4" class="py-5">
          <v-card-actions>
            <v-col cols="12" class="py-5">
              <v-row rows="5">
                <v-btn block color="green" dark size="small" prepend-icon="mdi-plus" @click="formPedidoPlato = true">
                  {{ action }}
                </v-btn>
              </v-row>
              <v-row rows="5">
                <!-- <v-btn block color="red" prepend-icon="mdi-cancel" dark size="small">
                  cancelar
                </v-btn> -->
              </v-row>
            </v-col>
          </v-card-actions>
        </v-col>
      </v-row>
    </v-card>
  
    <!-- Modal para la imagen -->
    <v-dialog v-model="dialog" max-width="600">
      <v-card>
        <v-img src="../assets/EQ.jpg" max-height="400" max-width="100%" cover></v-img>
      </v-card>
    </v-dialog>
  
    <!-- Modal para la información -->
    <v-dialog v-model="openInfo" max-width="600">
      <v-card size="bg">
        <v-row>
          <v-col cols="12">
            <v-card-title class="justify-center">
              <h1 class="headline" style="color:brown;">Información</h1>
            </v-card-title>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-card-text>
              <v-row>
                <v-col cols="6">
                  <h2 style="color:brown; padding: 12px;">Nombre del plato:</h2>
                </v-col>
                <v-col cols="6">
                  <h2 style="color:indigo; padding: 12px;">{{ title }}</h2>
                </v-col>
              </v-row>
            </v-card-text>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-card-text>
              <v-row>
                <v-col cols="6">
                  <h2 style="color:brown; padding: 12px;">Precio:</h2>
                </v-col>
                <v-col cols="6">
                  <h2 style="color:indigo; padding: 12px;">{{ subtitle }}</h2>
                </v-col>
              </v-row>
            </v-card-text>
          </v-col>
        </v-row>
        <v-btn icon class="dialog__content__wrapper" @click="openInfo = false"></v-btn>
      </v-card>
    </v-dialog>
  
    <!-- Modal para el formulario formFran -->
    <v-dialog v-model="formDialog" max-width="600">
      <v-card>
        <formFran @cancel="formDialog = false" @success="showSuccess" @error="showError"/>
      </v-card>
    </v-dialog>
  
    <!-- Modal de éxito -->
    <v-dialog v-model="successDialog" max-width="600">
      <v-card>
        <v-card-title class="headline">Éxito</v-card-title>
        <v-card-text>Formulario enviado con éxito.</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green" text @click="successDialog = false">Cerrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  
    <!-- Modal de error -->
    <v-dialog v-model="errorDialog" max-width="600">
      <v-card>
        <v-card-title class="headline">Error</v-card-title>
        <v-card-text>Error al enviar el formulario.</v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="errorDialog = false">Cerrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

      <!-- Modal vacío -->
      <v-dialog v-model="formPedidoPlato" max-width="600">
      <v-card>
        <v-card-title>Formulario de Pedido</v-card-title>
        <v-card-text>
            <form-food
          :dialog="formPedidoPlato"
          :name="title"
          :basePrice="parseFloat(subtitle.replace('€', ''))"
          @close="closeFormFood"
          @finalize="handleFinalize"
        />
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="closeFormFood">Cerrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </template>
  
  <script>
  import FormFran from '@/components/FormFran.vue';
  import FormFood from '@/components/FormFood.vue';
  
  export default {
    name: 'CardPhoto',
    components: {
      FormFran,
      FormFood
    },
    props: {
      title: {
        type: String,
        required: true
      },
      subtitle: {
        type: String,
        required: true
      },
      text: {
        type: String,
        required: true
      },
      action: {
        type: String,
        required: true
      },
      imgSrc: {
      type: String,
      required: true
    },
    emits: ['order-updated'],
    },
    data() {
      return {
        dialog: false,
        openInfo: false,
        formDialog: false, 
        successDialog: false, 
        errorDialog: false, 
        formPedidoPlato: false,
        

      }
    },
    methods: {
  openFormFood() {
    this.formPedidoPlato = true;
  },
  closeFormFood() {
    this.formPedidoPlato = false;
  },
  handleFinalize() {
    this.$emit('order-updated'); 
    this.closeFormFood();
  

    }
  }
    };

  </script>
  
  <style scoped>
  .image-container {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
.rounded-circle {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
  .dialog__content__wrapper {
    background-image: url('../assets/EQ.jpg');
    background-size: cover;
    background-position: center;
    width: 60px;
    height: 60px;
    position: absolute;
    top: 10px;
    right: 10px;
    border-radius: 50%;
    z-index: 1000;
    border: none;
  }
  .v-card-title {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .no-flex {
  flex: unset !important;
}

  </style>
  
