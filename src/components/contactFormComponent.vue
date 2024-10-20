<script setup>
import { computed, ref } from 'vue'

let contacts = ref([])
const name = ref('')
const number = ref('')
const newName = ref('')
const newNumber = ref('')
const contact1 = ref('')
const buttonRef = ref(null)
const updateButtonRef = ref(null);
const showModal = ref(false)


const saveContact = (contact) => {
    contact.preventDefault();
    contacts.value.push({
        name: name.value,
        number: number.value
    });
    name.value = ''
    number.value = ''
    newName.value = ''
    newNumber.value = ''
    contact1.value = ''
    console.log(contacts)
    active(buttonRef)
}

const filteredContacts = computed(() => {
    if (!contact1.value) return
    return contacts.value.filter(contact =>
        contact.name.toLowerCase().startsWith(contact1.value.toLowerCase())
    )
})

const deleteContact = (contact) => {
    contacts.value.splice(contact, 1)
    newName.value = ''
    newNumber.value = ''
    contact1.value = ''
    active(buttonRef)
    console.log(contacts)
}

const active = (button) => {
    if (button && button.value) {
        button.value.classList.add("is_active")
        setTimeout(() => {
            button.value.classList.remove("is_active")
        }, 1000)
    }
}

const closeModal = () => {
    showModal.value = false 
}
  
const updateContact = (contact) => {
    contacts.value.push({
        name: newName.value,
        number: newNumber.value
    });
    deleteContact(contact)
    console.log(newName.value)
    newName.value = ''
    newNumber.value = ''
    contact1.value = ''
    contact.preventDefault()
    closeModal()
    active(updateButtonRef)
    console.log(contacts)
}





</script>

<template>
    <div class="container">
        <h1>Contact book <span class="uli">by Uli</span></h1>
        <main>
            <div class="exp">
                <h3>This program is a basic contact book where users can add contacts by entering a name and phone number. The saved contacts are displayed in a list that can be filtered in real-time using a search input, with dynamic updates as the user types. It also allows updating and deleting contacts through specific buttons and a modal form for updates. Please note, this is an educational example; it's not intended for real data, phone numbers are not validated, and all data is stored only in the browser.</h3>
            </div>
            <form @submit="saveContact">
                <div class="inputs">
                    <input type="text" v-model="name" placeholder="Type your name..." maxlength="17" required>
                    <input type="number" v-model="number" placeholder="Type your number..." min="0" max="999999999" required>
                </div>
                <div class="wrapper">
                    <button ref="buttonRef" type="submit">
                        <span>Add contact</span>
                        <div class="success">
                            <svg xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" viewBox="0 0 29.756 29.756"
                                style="enable-background:new 0 0 29.756 29.756;" xml:space="preserve">
                                <path
                                    d="M29.049,5.009L28.19,4.151c-0.943-0.945-2.488-0.945-3.434,0L10.172,18.737l-5.175-5.173 c-0.943-0.944-2.489-0.944-3.432,0.001l-0.858,0.857c-0.943,0.944-0.943,2.489,0,3.433l7.744,7.752 c0.944,0.943,2.489,0.943,3.433,0L29.049,8.442C29.991,7.498,29.991,5.953,29.049,5.009z" />
                            </svg>
                        </div>
                    </button>
                </div>
            </form>
            <input type="text" v-model="contact1" placeholder="Search contacts by name..." class="filt">
            <p v-if="contact1 && filteredContacts.length === 0">No contacts found</p>
            <ul>
                <div class="contact">
                    <li v-for="contact in filteredContacts" :key="contact.name"
                        :title="`Contact name: ${contact.name}, Number: ${contact.number}`">
                        {{ contact.name }} <span>   {{ contact.number }}</span>
                        <button @click="showModal = true" class="modal-trigger">Update</button>
                        <button @click="deleteContact">Delete</button>
                    </li>
                </div>

                <div v-if="showModal" class="modal-wrap" @click.self="closeModal">
                    <div class="modal-bg"></div>
                    <div class="modal-content">
                        <div class="modal-header">
                            <span class="modal-title">Update contact</span>
                            <span class="modal-close" @click="closeModal">✖</span>
                        </div>
                        <div class="modal-body">
                            <form @submit="updateContact">
                            <div class="newInputs">
                                <input id="inputNm" type="text" v-model="newName" placeholder="Name" maxlength="17" required>
                                <input id="inputNb" type="number" v-model="newNumber" placeholder="Number" min="0" max="999999999" required>
                                <button ref="updateButtonRef" type="submit">Update contact</button>
                            </div>
                            </form> 
                        </div>
                    </div>
                </div>

            </ul>
        </main>
    </div>
</template>

<style scoped>
form {
    margin-bottom: 1rem;
}

.contact {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;

    & button {
        width: 100px;
    }
}

.inpts {
    display: flex;

}

main {
    width: 400px;
}

h3 {
    color: #61677C;
    opacity: 0.7;
    text-align: justify;
}

.inputs {
    display: flex;
    flex-wrap: nowrap;
}

.newInputs {
    display: flex;
    flex-direction: column;
    gap: 10px;
    & input {
        width: 350px;
    }
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
    display: flex;
    flex-wrap: wrap;
    align-items: baseline;
    gap: 5px;
    max-width: 500px;
    height: auto;
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

.wrapper {
    position: relative;
    width: 100%;
    height: 100%;
}

button {
    font-family: 'Geist Sans', sans-serif;
    ;
    position: relative;
    width: 170px;
    height: 40px;
    line-height: 1;
    font-size: 18px;
    font-weight: bold;
    border: 1px solid rgb(126, 91, 208);
    background: EBECF0;
    color: rgb(126, 91, 208);
    border-radius: 40px;
    cursor: pointer;
    overflow: hidden;
    transition: all .35s;
}

button:hover {
    background: #9f99dc;
    color: #fff;
}

button span {
    opacity: 1;
    visibility: visible;
    transition: all .35s;
}

.success {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #fff;
    border-radius: 50%;
    z-index: 1;
    opacity: 0;
    visibility: hidden;
    transition: all .35s;
}

.success svg {
    width: 20px;
    height: 20px;
    fill: yellowgreen;
    transform-origin: 50% 50%;
    transform: translateY(-50%) rotate(0deg) scale(0);
    transition: all .35s;
}

button.is_active {
    width: 40px;
    height: 40px;
}

button.is_active .success {
    opacity: 1;
    visibility: visible;
}

button.is_active .success svg {
    margin-top: 50%;
    transform: translateY(-50%) rotate(720deg) scale(1);
}

button.is_active span {
    opacity: 0;
    visibility: hidden;
}

body {
    background: #68b8c4;
    text-align: center;
    padding: 10%;
}

.modal-title {
    font-size: 25px;
}

.modal-wrap {
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 9999;
    background: rgba(0, 0, 0, 0.6);
}

.modal-content {
    background: white;
    border-radius: 10px;
    width: 400px;
    padding: 20px;
    position: relative;
    animation: fadeIn 0.4s ease-out;
}

.modal-close {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    font-size: 18px;
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

.modal-body {
    font-size: 16px;
    line-height: 1.5;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(-20px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}
</style>