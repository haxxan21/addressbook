<template>
    <div class="col-sm-12 border border-default p-3">
        <h2 class="p-2">
            Contacts
            <span class="float-right">
                <b-button v-b-modal.modal-1>Add Contact</b-button>
            </span>
        </h2>
        <table class="table">
            <thead>
                <th>Number</th>
                <th>Image</th>
                <th>Name</th>
                <th>Email</th>
                <th>Address</th>
                <th>Created</th>
                <th>Action</th>
            </thead>
            <ContactList
                :contact="contacts"
                @clicked="deleteRow"
                @clicker="editRow"
            />
        </table>
        <Modal @clicked="clicked" />
        <EditModal :row="edit" @updated="update" />
    </div>
</template>
<script>
import ContactList from "../components/TableRow";
import EditModal from "../components/EditModal";
import Modal from "../components/Modal";
import { BButton } from "bootstrap-vue";

export default {
    components: {
        ContactList,
        Modal,
        EditModal
    },
    data() {
        return {
            contacts: [],
            fullname: "",
            email: "",
            address: "",
            delete: "",
            edit: ""
        };
    },
    mounted() {
        this.getContacts();
    },
    methods: {
        getContacts: function() {
            axios
                .get("/contact")
                .then(response => (this.contacts = response.data));
        },

        addContact: function() {
            axios
                .post("/contact", {
                    fullname: this.fullname,
                    email: this.email,
                    address: this.address
                })
                .then(response => (this.success = response.data));
        },

        clicked: function(value) {
            this.getContacts();
        },

        deleteRow: function(id) {
            if (id) {
                axios
                    .delete("/contact/" + id)
                    .then(response => (this.delete = response.data));
            }
            this.getContacts();
        },

        editRow: function(id) {
            axios
                .get("/contact/" + id)
                .then(response => (this.edit = response.data));
        },

        update: function(id) {
            this.getContacts();
        }
    }
};
</script>
