<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContactParams: {},
      currentContact: {},
      editContactParams: {}
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get(`/contacts`).then(response => {
        console.log("finding contacts", response)
        this.contacts = response.data
      })
    },
    createContact: function () {
      axios.post(`/contacts`, this.newContactParams).then(response => {
        console.log('creating contact', response)
        this.contacts.push(response.data)
        this.newContactParams = {}
      })
    },
    showContact: function (contact) {
      this.currentContact = contact;
      this.editContactParams = contact;
      document.querySelector('#contact-details').showModal();
    },
    updateContact: function (contact) {
      axios.patch(`/contacts/${contact.id}`, this.editContactParams).then(response => {
        console.log("updating contact", response),
          this.currentContact = {};
      })
    },
    destroyContact: function (contact) {
      axios.delete(`/contacts/${contact.id}`).then(response => {
        console.log("contact deleted", response);
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>{{ contact.first_name }} {{ contact.last_name }}</p>
      <button v-on:click="showContact(contact)">More Info</button>
      <dialog id="contact-details">
        <form method="dialog">
          <p>First Name: {{ currentContact.first_name }}</p>
          <p>Last Name: {{ currentContact.last_name }}</p>
          <p>Email: {{ currentContact.email }}</p>
          <p>Phone: {{ currentContact.phone_number }}</p>
          <p>Edit Information</p>
          <p>
            First Name:
            <input type="text" v-model="editContactParams.first_name" />
          </p>
          <p>
            Last Name:
            <input type="text" v-model="editContactParams.last_name" />
          </p>
          <p>
            Email:
            <input type="text" v-model="editContactParams.email" />
          </p>
          <p>
            Phone:
            <input type="text" v-model="editContactParams.phone_number" />
          </p>
          <button v-on:click="updateContact(currentContact)">Update Info</button>
          <button v-on:click="destroyContact(currentContact)">Delete</button>
          <button>Close</button>
        </form>
      </dialog>
    </div>
    <p>Add Contact</p>
    <p>
      First Name:
      <input type="text" v-model="newContactParams.first_name" />
    </p>
    <p>
      Last Name:
      <input type="text" v-model="newContactParams.last_name" />
    </p>
    <p>
      email:
      <input type="text" v-model="newContactParams.email" />
    </p>
    <p>
      Phone:
      <input type="text" v-model="newContactParams.phone_number" />
    </p>
    <button v-on:click="createContact()">Add Contact</button>
  </div>
</template>

<style></style>