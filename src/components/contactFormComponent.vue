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
    <div class="container">
    <h1>Contact book <span class="uli">by Uli</span></h1>
    <main>
        <form>
            <input type="text" v-model="name" placeholder="Write your name..." required >
            <input type="text" v-model="number" placeholder="Write your number..." required >
            <button @click="saveContact">Save contact</button>
            <input type="text" v-model="contact1" placeholder="Filter contacts" class="filt">
        <ul>
            <li v-for="contact in filteredContacts" :key="contact.name"  
            :title="`Contact name: ${contact.name}, Number: ${contact.number}`">
            {{ contact.name }} - <span>{{ contact.number }}</span></li>
        </ul>
        </form>
    </main>
</div>
</template>

<style scoped>
    div {
        min-height: 90vh;
    }

    .container {
        display: grid;
        place-content: center;
        flex-direction: column;
        align-items: center;
    }
    main {
        display: flex;
        flex-direction: column;
        align-items: center;
        
    }
    li {
        list-style: none;
        font-size: 20px;
        color: #61677C;

    }

    span {
        color: rgb(126, 91, 208);
        font-weight: bolder;
    }

    .filt {
        width: 325px;
    }

    .uli {
        color: rgb(126, 91, 208);
        font-weight: bolder; 
        font-size: 23px;
        font-style: italic;
    }
</style>