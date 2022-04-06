<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Contacts!",
      contacts: [],
      newContactParams: {},
      currentContact: {},
      updateContactParams: {}
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      console.log("getting contacts")
      axios.get("/contacts").then(response => {
        console.log(response.data)
        this.contacts = response.data
      })
    },
    createContact: function () {
      axios.post("/contacts", this.newContactParams).then(response => {
        console.log("making contact", response);
        this.contacts.push(response.data)
        this.newContactParams = {}
      })
    },
    showContact: function (contact) {
      this.currentContact = contact
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      axios.patch(`/contacts/${contact.id}`).then(response => {
        console.log("updating contact", response);
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>Name: {{ contact.first_name }} {{ contact.last_name }} phone: {{ contact.phone_number }} email: {{ contact.email }}</p>
      <button v-on:click="showContact(contact)">More info</button>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <p>First Name: {{ currentContact.first_name }}</p>
        <p>Last Name: {{ currentContact.last_name }}</p>
        <p>Email: {{ currentContact.email }}</p>
        <p>Phone: {{ currentContact.phone_number }}</p>
        <p>{{ currentContact.image }}</p>
        <p>
          First Name:
          <input type="text" v-model="updateContactParams.first_name" />
        </p>
        <p>
          Last Name:
          <input type="text" v-model="updateContactParams.last_name" />
        </p>
        <p>
          Email:
          <input type="text" v-model="updateContactParams.email" />
        </p>
        <p>
          Phone:
          <input type="text" v-model="updateContactParams.phone_number" />
        </p>
        <p>
          Image:
          <input type="text" v-model="updateContactParams.image" />
        </p>
        <button v-on:click="updateContact(currentContact)">Update</button>
        <button>Close</button>
      </form>
    </dialog>
    <p>Add Contact:</p>
    <p>
      First Name:
      <input type="text" v-model="newContactParams.first_name" />
    </p>
    <p>
      Last Name:
      <input type="text" v-model="newContactParams.last_name" />
    </p>
    <p>
      Email:
      <input type="text" v-model="newContactParams.email" />
    </p>
    <p>
      Phone:
      <input type="text" v-model="newContactParams.phone_number" />
    </p>
    <p>
      Image:
      <input type="text" v-model="newContactParams.image" />
    </p>
    <button v-on:click="createContact()">Create Contact</button>
  </div>
</template>

<style></style>