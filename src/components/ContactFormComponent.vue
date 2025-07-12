<template>
  <div>
    <h2>{{ contact.id ? 'Editar Contacto' : 'Agregar Contacto' }}</h2>
    <form @submit.prevent="submitForm">
      <input v-model="contact.name" placeholder="Nombre" required />
      <input v-model="contact.email" placeholder="Email" required />
      <input v-model="contact.address" placeholder="Dirección" />
      <input v-model="contact.phone" placeholder="Teléfono" />
      <input v-model="contact.country" placeholder="País" />
      <input v-model="contact.city" placeholder="Ciudad" />
      <button type="submit">
        <i :class="contact.id ? 'fas fa-save' : 'fas fa-plus'"></i>
        {{ contact.id ? 'Actualizar' : 'Agregar' }}
      </button>

      <button v-if="contact.id" type="button" @click="resetForm">Cancelar</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['editContact'],
  emits: ['refresh-data', 'clear-edit'],
  data() {
    return {
      contact: this.getEmptyContact()
    }
  },
  watch: {
    editContact(newVal) {
      this.contact = { ...newVal }
    }
  },
  methods: {
    getEmptyContact() {
      return {
        name: '',
        email: '',
        address: '',
        phone: '',
        country: '',
        city: ''
      }
    },
    async submitForm() {
      if (this.contact.id) {
        await axios.put(`http://localhost:3000/contactos/${this.contact.id}`, this.contact) // Si el contacto tiene ID, actualiza
      } else {
        await axios.post('http://localhost:3000/contactos', this.contact) // Si no tiene ID, crea un nuevo contacto
      }
      // Finalmente, se refresca la lista de contactos y resetea el formulario
      this.$emit('refresh-data')
      this.resetForm()
    },
    resetForm() {
      // Resetea el formulario a un contacto vacío y emite el evento para limpiar la edición
      this.contact = this.getEmptyContact()
      this.$emit('clear-edit')
    }
  }
}
</script>

<style scoped>
form {
  background-color: #ffffff;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  margin-bottom: 2rem;
}

input {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  box-sizing: border-box;
  font-size: 14px;
}

button {
  margin-top: 16px;
  padding: 10px 16px;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
}

button[type="submit"] {
  background-color: #3b82f6;
  color: white;
  margin-right: 8px;
}

button[type="button"] {
  background-color: #e5e7eb;
  color: #111827;
}
</style>
