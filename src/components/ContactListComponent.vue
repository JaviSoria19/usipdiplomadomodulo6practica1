<template>
  <div>
    <h2>Lista de Contactos (el ID solo se muestra con fines de desarrollo)</h2>
    <input v-model="search" placeholder="Escribe aquí para buscar por nombre, email o cualquier otro campo"
      class="input" />
    <ul>
      <li v-for="contact in filteredContacts" :key="contact.id">
        <span>{{ contact.id }} <i class="fas fa-user"></i></span> <strong>{{ contact.name }}</strong> - <i class="fas fa-envelope"></i> {{ contact.email }}
        <br />
        <i class="fas fa-phone"></i> {{ contact.phone }}
        <br>
        <i class="fas fa-location-dot"></i> {{ contact.address }} - <i class="fas fa-city"></i> {{ contact.city }}, <i class="fas fa-globe"></i> {{ contact.country }}
        <br />
        <button @click="$emit('edit-contact', contact)">
          <i class="fas fa-pen"></i> Editar
        </button>

        <button @click="deleteContact(contact.id)">
          <i class="fas fa-trash"></i> Eliminar
        </button>

        <hr />
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['contacts'],
  emits: ['refresh-data', 'edit-contact'],
  data() {
    return {
      search: ''
    }
  },
  computed: {
    // Propiedad computada para filtrar los contactos por cualquier campo
    filteredContacts() {
      const cadenaDeBusqueda = this.search.toLowerCase().trim()
      if (!cadenaDeBusqueda) return this.contacts

      return this.contacts.filter(contact => {
        return Object.values(contact).some(value =>
          String(value).toLowerCase().includes(cadenaDeBusqueda)
        )
      })
    }
  },
  methods: {
    async deleteContact(id) {
      if (confirm('¿Estás seguro de eliminar este contacto?')) {
        await axios.delete(`http://localhost:3000/contactos/${id}`)
        this.$emit('refresh-data')
      }
    }
  }
}
</script>

<style scoped>
.input {
  margin-bottom: 16px;
  padding: 10px;
  width: 98%;
  font-size: 14px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: #ffffff;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

strong {
  font-size: 16px;
  color: #1f2937;
}

button {
  margin-top: 8px;
  margin-right: 8px;
  padding: 8px 12px;
  font-size: 13px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
}

button:nth-of-type(1) {
  background-color: #f59e0b;
  color: white;
}

button:nth-of-type(2) {
  background-color: #ef4444;
  color: white;
}

span {
  color: #3b82f6;
  font-weight: bold;
}
</style>