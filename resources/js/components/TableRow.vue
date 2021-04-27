<template>
    <tbody>
        <tr v-for="(con, index) in contact" :key="index">
            <td>{{ index + 1 }}</td>
            <td><img :src="'images/' + con.image" alt="" width="35px" /></td>
            <td>{{ con.fullname }}</td>
            <td>{{ con.email }}</td>
            <td>{{ con.address }}</td>
            <td>{{ con.created }}</td>
            <td>
                <a @click="deleteRow(con.id)" class="link"
                    ><i class="fa fa-trash" aria-hidden="true"></i
                ></a>
                <a @click="editRow(con.id)" class="link">
                    <i class="fa fa-pencil-square" aria-hidden="true"></i>
                </a>
            </td>
        </tr>

        <b-modal id="confirmation-modal" hide-footer class="p-3">
            <h5 class="text-center p-2 mb-5">
                Are you sure you want to delete this contact?
            </h5>
            <div class="row text-center mb-5">
                <div class="col-sm-6">
                    <button class="btn btn-primary btn-lg" @click="hideModal">
                        Cancel
                    </button>
                </div>
                <div class="col-sm-6">
                    <button
                        @click="deleteContact"
                        class=" btn-lg btn btn-danger"
                    >
                        Delete
                    </button>
                </div>
            </div>
        </b-modal>
    </tbody>
</template>
<script>
import { BModal, Bbutton } from "bootstrap-vue";
export default {
    props: ["contact"],
    mounted() {
        console.log("Component mounted.");
    },
    data() {
        return {
            id: "",
            editRows: ""
        };
    },
    methods: {
        deleteRow: function(id) {
            this.id = id;
            this.$bvModal.show("confirmation-modal");
        },
        deleteContact: function() {
            this.$emit("clicked", this.id);
            this.$bvModal.hide("confirmation-modal");
        },
        hideModal: function() {
            this.$bvModal.hide("confirmation-modal");
        },
        editRow: function(id) {
            this.$emit("clicker", id);
            this.$bvModal.show("modal-5");
        }
    }
};
</script>

<style scoped>
.fa {
    cursor: pointer;
    font-size: 23px;
}
.link {
    text-decoration: none;
}
.fa-trash {
    color: red;
}
.fa-pencil-square {
    color: blue;
}
</style>
