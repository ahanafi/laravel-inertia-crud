<template>
    <div>
        <div class="card">
            <div class="card-header">
                <h4 class="card-title">
                    List Contacts
                    <button @click="showContactForm" class="btn btn-sm btn-primary float-end" type="button">
                        <i class="fa fa-plus"></i>
                        <span>Add Contact</span>
                    </button>

                </h4>
            </div>
            <div class="card-body">
                <table class="table table-bordered table-striped">
                    <thead>
                    <tr>
                        <th>#</th>
                        <th>Name</th>
                        <th>Phone</th>
                        <th>Address</th>
                        <th>Actions</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(contact, index) in contacts">
                        <td class="text-center">{{ ++index }}</td>
                        <td>{{ contact.name }}</td>
                        <td>{{ contact.phone }}</td>
                        <td>{{ contact.address }}</td>
                        <td>
                            <button type="button" @click="editContact(contact)" class="btn btn-sm btn-secondary">
                                <i class="fa fa-pencil-alt"></i>
                                <span>Edit</span>
                            </button>
                            <button type="button" @click="confirmDelete(contact)" class="btn btn-sm btn-danger">
                                <i class="fa fa-trash"></i>
                                <span>Delete</span>
                            </button>
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <div class="modal fade" id="contact-form" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel" v-if="!isDelete">Contact Form</h5>
                        <h5 class="modal-title" v-if="isDelete">Confirm Delete</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <form action="" v-on:submit="saveContact" v-if="!isDelete">
                            <div class="mb-3 row">
                                <label class="col-sm-3 col-form-label text-right">Name</label>
                                <div class="col-sm-9">
                                    <input type="text" class="form-control has-validation" v-model="contact.name"
                                           placeholder="Put name here...">
                                    <div class="invalid-feedback" v-if="errors.name">
                                        {{ errors.name }}
                                    </div>
                                </div>
                            </div>

                            <div class="mb-3 row">
                                <label class="col-sm-3 col-form-label text-right">Phone Number</label>
                                <div class="col-sm-9">
                                    <input type="tel" class="form-control has-validation" v-model="contact.phone"
                                           placeholder="Put phone number here...">
                                    <div class="invalid-feedback" v-if="errors.phone">
                                        {{ errors.phone }}
                                    </div>
                                </div>
                            </div>

                            <div class="mb-3 row">
                                <label class="col-sm-3 col-form-label text-right">Address</label>
                                <div class="col-sm-9">
                                    <input type="text" class="form-control" v-model="contact.address"
                                           placeholder="Optional form">
                                </div>
                            </div>
                        </form>
                        <div v-if="isDelete">
                            <div class="alert alert-danger" role="alert">
                                Are your sure want to delete this?
                                The data couldn't be recovered!
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
                        <button @click="saveContact" v-if="!editMode && !isDelete" type="button"
                                class="btn btn-primary">Save Contact
                        </button>
                        <button @click="updateContact" v-if="editMode && !isDelete" type="button"
                                class="btn btn-primary">Update Contact
                        </button>
                        <button @click="deleteContact" v-if="!editMode && isDelete" type="button"
                                class="btn btn-danger">Delete Now
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Layout from "../../Shared/Layout";

export default {
    name: "Contacts",
    layout: Layout,
    data() {
        return {
            contact: {
                id: null,
                name: null,
                phone: null,
                address: null
            },
            editMode: false,
            isDelete: false,
        }
    },
    props: [
        "contacts", "errors"
    ],
    methods: {
        showContactForm() {
            const contactForm = document.getElementById("contact-form");
            const modalContactForm = new bootstrap.Modal(contactForm);
            modalContactForm.show();
        },
        hideContactForm() {
            const contactForm = document.getElementById("contact-form");
            const modalContactForm = new bootstrap.Modal(contactForm);
            modalContactForm.hide()
            this.resetForm();
        },
        saveContact() {
            this.$inertia.post('/contacts', this.contact);
            this.editMode = false;
            this.hideContactForm();
        },
        editContact(contact) {
            this.contact = Object.assign({}, contact);
            this.editMode = true;
            this.showContactForm();
        },
        updateContact() {
            this.$inertia.put(`/contacts/${this.contact.id}`, this.contact);
            this.hideContactForm();
        },
        deleteContact() {
            this.$inertia.delete(`/contacts/${this.contact.id}`, this.contact);
            this.hideContactForm();
        },
        resetForm() {
            this.contact = {
                name: '',
                phone: '',
                address: ''
            }
        },
        confirmDelete(contact) {
            this.contact = Object.assign({}, contact);
            this.editMode = false;
            this.isDelete = true;
            this.showContactForm();
        }
    }
}
</script>

<style scoped>

</style>
