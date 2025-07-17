<template>
  <div>
    <h1>Gestión de Contactos</h1>
    <ContactFormComponent :editContact="editContact" @refresh-data="fetchContacts" @clear-edit="editContact = null" />
    <ContactListComponent :contacts="contacts" @refresh-data="fetchContacts" @edit-contact="editContact = $event" />
  </div>
</template>

<script>
import axios from 'axios'
import ContactFormComponent from '../components/ContactFormComponent.vue'
import ContactListComponent from '../components/ContactListComponent.vue'

export default {
  components: {
    ContactFormComponent,
    ContactListComponent
  },
  data() {
    return {
      contacts: [],
      editContact: null
    }
  },
  created() {
    this.fetchContacts()
  },
  methods: {
    // Método para obtener la lista de contactos desde la API de JSON Server mediante Axios
    async fetchContacts() {
      try {
        const response = await axios.get('http://localhost:3000/contactos')
        this.contacts = response.data
      } catch (error) {
        console.error('Error al obtener los contactos:', error)
        alert('Ocurrió un error al cargar los contactos.')
      }
    }
  }
}
</script>

<style scoped>
h1 {
  text-align: center;
}
</style>