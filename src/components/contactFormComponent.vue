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
        <div class="exp">
        <h3>This basic program is a contact book where users can add contacts by entering a name and phone number. It displays the saved contacts in a list and allows users to filter them in real-time using a search input. The filtered results update dynamically as the user types.</h3>
        </div>
        <form @submit="saveContact">
            <div class="inputs">
            <input type="text" v-model="name" placeholder="Write your name..." required >
            <input type="number" v-model="number" placeholder="Write your number..." required minlength=9>
            </div>
            <button type="submit">Save contact</button>
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
    main {
        width: 400px;

    }

    h3 {
       color:#61677C;
       opacity: 0.7;
    }

    .inputs {
        display: flex;
        flex-wrap: nowrap;
    }
    .container {
        display: grid;
        place-content: center;
        place-items: center;
        place-self: center;
        justify-items: end;
        height: 90vh;
        ;
    }

    .exp {
        max-width: 400px;
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
        width: 400px;
    }

    .uli {
        color: rgb(126, 91, 208);
        font-weight: bolder; 
        font-size: 23px;
        font-style: italic;
    }

</style>