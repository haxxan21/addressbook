<template>
    <div>
        <b-modal id="modal-1" hide-footer title="Add Contact">
            <form
                ref="addForm"
                action=""
                @submit.prevent="clicked"
                enctype="multipart/form-data"
            >
                <div class="row">
                    <div class="col-sm-12 mb-2">
                        <label for="">Full Name</label>
                        <input
                            type="text"
                            v-model="fullname"
                            class="form-control"
                        />
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Email</label>
                        <input
                            type="email"
                            v-model="email"
                            class="form-control"
                        />
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Address</label>
                        <textarea
                            type="email"
                            v-model="address"
                            class="form-control"
                        ></textarea>
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Upload Image</label>
                        <input
                            type="file"
                            @change="loadFile"
                            class="form-control"
                            id="file"
                        />
                    </div>
                    <div class="col-sm-12">
                        <button class="btn btn-success">Submit</button>
                    </div>
                </div>
            </form>
        </b-modal>
    </div>
</template>
<script>
import { BModal } from "bootstrap-vue";

export default {
    mounted() {
        console.log("moodal mounted");
        this.formdata = new FormData();
    },
    data() {
        return {
            fullname: "",
            email: "",
            address: "",
            formdata: {}
        };
    },
    methods: {
        clicked: function() {
            this.formdata.append("fullname", this.fullname);
            this.formdata.append("email", this.email);
            this.formdata.append("address", this.address);
            axios
                .post("/contact", this.formdata)
                .then(response => (this.success = response.data))
                .then(data => {
                    this.$emit("clicked", true);
                });

            this.resetData();
            this.$bvModal.hide("modal-1");
        },
        loadFile: function() {
            let loadFile = document.getElementById("file");
            this.formdata.append("file", loadFile.files[0]);
        },
        resetData: function() {
            this.fullname = "";
            this.email = "";
            this.address = "";
        }
    }
};
</script>
