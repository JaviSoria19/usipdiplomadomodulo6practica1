<template>
  <div>
    <h2>Lista de Contactos</h2>
    <label for="search"><i class="fas fa-search"></i> Búsqueda:</label>
    <input id="search" v-model="search" placeholder="Escribe aquí para buscar por nombre, email o cualquier otro campo"
      class="input" />

    <h3><i class="fas fa-filter"></i> Filtros:</h3>
    <div class="filters">
      <label><i class="fas fa-user"></i> Nombre:</label>
      <input v-model="filters.name" placeholder="Filtrar por nombre" />

      <label><i class="fas fa-envelope"></i>Email:</label>
      <input v-model="filters.email" placeholder="Filtrar por email" />

      <label><i class="fas fa-location-dot"></i> Dirección:</label>
      <input v-model="filters.address" placeholder="Filtrar por dirección" />

      <label><i class="fas fa-phone"></i> Teléfono:</label>
      <input v-model="filters.phone" placeholder="Filtrar por teléfono" />

      <label><i class="fas fa-globe"></i> País:</label>
      <input v-model="filters.country" placeholder="Filtrar por país" />

      <label><i class="fas fa-city"></i> Ciudad:</label>
      <input v-model="filters.city" placeholder="Filtrar por ciudad" />
    </div>

    <ul>
      <li v-for="contact in filteredContacts" :key="contact.id">
        <span>{{ contact.id }} <i class="fas fa-user"></i></span> <strong>{{ contact.name }}</strong> - <i
          class="fas fa-envelope"></i> {{ contact.email }}
        <br />
        <i class="fas fa-location-dot"></i> {{ contact.address }}
        <br>
        <i class="fas fa-phone"></i> {{ contact.phone }}
        <br>
        <i class="fas fa-globe"></i> {{ contact.country }} <i class="fas fa-city"></i> {{ contact.city }}
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
      search: '',
      filters: {
        name: '',
        email: '',
        address: '',
        phone: '',
        country: '',
        city: ''
      }
    }
  }
  ,
  computed: {
    filteredContacts() {
      return this.contacts.filter(contact => {
        // Búsqueda general
        const searchMatch = this.search
          ? Object.values(contact).some(value =>
            String(value).toLowerCase().includes(this.search.toLowerCase())
          )
          : true

        // Filtros específicos
        const fieldFiltersMatch = Object.entries(this.filters).every(([key, value]) => {
          return value === '' || contact[key].toLowerCase().includes(value.toLowerCase())
        })

        return searchMatch && fieldFiltersMatch
      })
    }
  }
  ,
  methods: {
    async deleteContact(id) {
      try {
        if (confirm('¿Estás seguro de eliminar este contacto?')) {
          await axios.delete(`http://localhost:3000/contactos/${id}`)
          this.$emit('refresh-data')
        }
      } catch (error) {
        console.error(`Error al eliminar el contacto con ID ${id}:`, error)
        alert('Ocurrió un error al eliminar el contacto.')
      }
    }

  }
}
</script>

<style scoped>
.input {
  margin-bottom: 16px;
  padding: 10px;
  width: 86.5%;
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

label {
  font-weight: bold;
  margin-right: 10px;
  color: #00c180;
}

.filters input {
  margin-bottom: 8px;
  padding: 8px;
  width: 98%;
  font-size: 14px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
}

.filters label {
  display: block;
  margin: 10px;
}
</style>