<template>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="Email"
              hide-details
              required
            ></v-text-field>
          </v-col>
  
          <v-col cols="12" md="4">
            <v-text-field
              v-model="telephone"
              :counter="9"
              :rules="telephoneRules"
              label="Teléfono"
              hide-details
              required
              type="number"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-textarea
              v-model="message"
              label="Pedidos On-line"
              placeholder="Escribe tu pedido aquí"
              required
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" class="d-flex justify-end">
            <v-btn @click="cancelForm" color="red" class="mr-4">Cancelar</v-btn>
            <v-btn @click="submitForm" :disabled="!valid" color="green">Enviar</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </template>
  
  <script>
  import emailjs from 'emailjs-com';
  
  export default {
    data: () => ({
      valid: false,
      email: '',
      message: '',
      telephone: '',
      telephoneRules: [
        value => {
          if (value) return true;
          return 'Teléfono es obligatorio.';
        },
        value => {
          if (value?.length >= 9) return true;
          return 'El teléfono tiene que tener al menos 9 dígitos.';
        },
      ],
      emailRules: [
        value => {
          if (value) return true;
          return 'El correo electrónico es obligatorio.';
        },
        value => {
          if (/.+@.+\..+/.test(value)) return true;
          return 'El correo electrónico debe ser válido.';
        },
      ],
    }),
    methods: {
      resetForm() {
        this.telephone = '';
        this.email = '';
        this.message = '';
        this.valid = false;
      },
      cancelForm() {
        this.resetForm();
        this.$emit('cancel');
      },
      submitForm() {
        if (this.valid) {
          const templateParams = {
            from_name: this.telephone,
            email: this.email,
            message: this.message,
          };
  
          emailjs.send('service_mak3mee', 'template_u6xkhiw', templateParams, '_LdBzq4DpPSh-4UNS')
            .then((response) => {
              console.log('SUCCESS!', response.status, response.text);
              this.$emit('success');
            }, (error) => {
              console.log('FAILED...', error);
              this.$emit('error');
            });
  
          this.resetForm();
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .mr-4 {
    margin-right: 16px;
  }
  </style>
  