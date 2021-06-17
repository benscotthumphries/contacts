<template>
  <div id="app">
    <div>
      <h1>Contacts:</h1>
      <li v-for="(contact, n) in contacts" :key="contact.id">
        <label class="view">{{ contact.name }}</label>
        <label class="view">{{ contact.address }}</label>
        <label class="view">{{ contact.phone }}</label>
        <label class="view">{{ contact.email }}</label>
        <label class="view">{{ contact.category }}</label>
        <p class="view">
          <button @click="removeContact(n)">Remove</button>
        </p>
        <form id='myForm' v-if='show' v-on:submit.prevent="editContact(n)">
              <input class="edit" v-model="newName" placeholder="Name">
              <input class="edit" v-model="newAddress" placeholder="Address">
              <input class="edit" v-model="newPhone" placeholder="Phone">
              <input class="edit" v-model="newEmail" placeholder="Email">
              <input class="edit" v-model="newCategory" placeholder="Category">
              <button  @click='editContact(n)'>Submit Edit</button>
        </form>
      </li>
    </div>
    <button  @click='showContact()'>Edit</button>
    <button  @click='hideContact()'>Stop Editing</button>
    <div>
      <h2>Add Contact:</h2>
      <form id='myForm'>
        <input v-model="name" placeholder="Name">
        <input v-model="address" placeholder="Address">
        <input v-model="phone" placeholder="Phone">
        <input v-model="email" placeholder="Email">
        <input v-model="category" placeholder="Category">
        <button @click="addContact">Add Contact</button>
      </form>
    </div>
  </div>
</template>
<script>


export default {
  name: 'app',
  data() {
    return {
      contacts: {},
      show: false,
      address: '',
      name: '',
      category: '',
      phone: '',
      email: '',
    }
  },
  mounted() {
    if (localStorage.getItem('contacts')) {
      try {
        this.contacts = JSON.parse(localStorage.getItem('contacts'));
      } catch(e) {
        localStorage.removeItem('contacts');
      }
    }
  },
  methods: {
    addContact() {
      this.contacts.push({name: this.name, address: this.address, phone: this.phone, email: this.email, category: this.category});
      this.saveContacts();
      var form = document.getElementById("myForm");
      form.reset();
    },
    removeContact(n) {
      this.contacts.splice(n, 1);
      this.saveContacts();
    },
    saveContacts() {
      const parsed = JSON.stringify(this.contacts);
      localStorage.setItem('contacts', parsed);
    },
    editContact(n){
      this.contacts[n].name = this.newName;
      this.contacts[n].address = this.newAddress;
      this.contacts[n].phone = this.newPhone;
      this.contacts[n].email = this.newEmail;
      this.contacts[n].category = this.newCategory;
      this.saveContacts();
      location.reload();
    },
    showContact(){
      this.show = true;
    },
    hideContact(){
      this.show = false;
    },
  },
};


</script>

<style scoped>

#show {
  display: none;
}
.view{
  padding:10px;
  width:60%;
}

ul {
  list-style: none;
}

li {
    width: 100%;
    min-height: 30px;
    padding: 10px;
    margin-bottom: 10px;
    font-size: 1em;
    display: flex;
    align-items: center;
}

.delete {
    display: none;
    margin-left: auto;
}

</style>