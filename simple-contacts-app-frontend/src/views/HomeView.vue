<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Contacts!",
      contacts: [],
      newContact: {},
      currentContact: {},
      editContactParams: {}
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then(response => {
        console.log("finding contacts", response.data),
          this.contacts = response.data
      })
    },
    createContact: function () {
      axios.post("/contacts", this.newContact).then(response => {
        console.log("creating contact", response)
        this.contacts.push(response.data)
      })
    },
    showContact: function (contact) {
      this.currentContact = contact;
      this.editContactParams = contact;
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      axios.patch(`/contacts/${contact.id}`, this.editContactParams).then(response => {
        console.log("updating contact", response);
        this.currentContact = {};
      })
    },
    destroyContact: function (contact) {
      axios.delete(`/contacts/${contact.id}`).then(response => {
        console.log("deleted contact", response)
      })
    }
  }
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <p>Name: {{ contact.first_name }} {{ contact.last_name }}</p>
      <button v-on:click="showContact(contact)">More Information</button>
      <dialog id="contact-details">
        <form method="dialog">
          <p>First Name: {{ currentContact.first_name }}</p>
          <p>Last Name: {{ currentContact.last_name }}</p>
          <p>Email: {{ currentContact.email }}</p>
          <p>Phone Number: {{ currentContact.phone_number }}</p>
          <p>Edit Contact</p>
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
            Phone Number:
            <input type="text" v-model="editContactParams.phone_number" />
          </p>
          <button v-on:click="updateContact(currentContact)">Update</button>
          <button v-on:click="destroyContact(currentContact)">Delete</button>
          <button>Close</button>
        </form>
      </dialog>
    </div>
    <p>Create New Contact</p>
    <p>
      First Name:
      <input type="text" v-model="newContact.first_name" />
    </p>
    <p>
      Last Name:
      <input type="text" v-model="newContact.last_name" />
    </p>
    <p>
      Phone Number:
      <input type="text" v-model="newContact.phone_number" />
    </p>
    <p>
      email:
      <input type="text" v-model="newContact.email" />
    </p>
    <button v-on:click="createContact()">Create</button>
  </div>
</template>

<style></style>