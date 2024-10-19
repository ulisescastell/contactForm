<script setup>
    import { computed, ref } from 'vue'

    let contacts = ref([])
    const name = ref('')
    const number = ref('')
    const contact1 = ref('')

    const saveContact = (contact) => {
       contact.preventDefault();
       contacts.value.push({
        name: name.value,
        number: number.value
       });
       name.value = ''
       number.value = ''
       console.log(contacts)
    } 

    const filteredContacts = computed(() => {
        if(!contact1.value) return
        return contacts.value.filter(contact => 
            contact.name.toLowerCase().startsWith(contact1.value.toLowerCase())
        )
    })
</script>

<template>
    <main>
        <form>
            <input type="text" v-model="name" placeholder="Write your name...">
            <input type="text" v-model="number" placeholder="Write your number...">
            <button @click="saveContact">Save contact</button>
        </form>
        <input type="text" v-model="contact1" placeholder="Filter contacts">
        <ul>
            <li v-for="contact in filteredContacts" :key="contact.name"  
            :title="`Contact name: ${contact.name}, Number: ${contact.number}`">
            {{ contact.name }} - {{ contact.number }}</li>
        </ul>
    </main>
</template>

<style scoped>

</style>