<script setup>
import { computed, ref } from 'vue'

let contacts = ref([])
const name = ref('')
const number = ref('')
const contact1 = ref('')
const buttonRef = ref(null)

const saveContact = (contact) => {
    contact.preventDefault();
    contacts.value.push({
        name: name.value,
        number: number.value
    });
    name.value = ''
    number.value = ''
    console.log(contacts)
    active()
}

const filteredContacts = computed(() => {
    if (!contact1.value) return
    return contacts.value.filter(contact =>
        contact.name.toLowerCase().startsWith(contact1.value.toLowerCase())
    )
})

//button
const active = () => {
    if (buttonRef.value) {
        buttonRef.value.classList.add("is_active")
        setTimeout(() => {
            buttonRef.value.classList.remove("is_active")
        }, 1000)
    }
}



</script>

<template>
    <div class="container">
        <h1>Contact book <span class="uli">by Uli</span></h1>
        <main>
            <div class="exp">
                <h3>This basic program is a contact book where users can add contacts by entering a name and phone
                    number. It displays the saved contacts in a list and allows users to filter them in real-time using
                    a search input. The filtered results update dynamically as the user types.</h3>
            </div>
            <form @submit="saveContact">
                <div class="inputs">
                    <input type="text" v-model="name" placeholder="Write your name..." required>
                    <input type="number" v-model="number" placeholder="Write your number..." required>
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
    color: #61677C;
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

.wrapper {
    position: relative;
    width: 100%;
    height: 100%;
}

button {
    font-family: 'Geist Sans', sans-serif;;
    position: relative;
    width: 170px;
    height: 40px;
    line-height: 1;
    font-size: 18px;
    font-weight: bold;
    border: 3px solid rgb(126, 91, 208);
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
</style>