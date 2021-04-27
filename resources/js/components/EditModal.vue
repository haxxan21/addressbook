<template>
    <div>
        <b-modal id="modal-5" hide-footer title="Edit Contact">
            <form
                ref="editForm"
                action=""
                @submit.prevent="update"
                enctype="multipart/form-data"
            >
                <div class="row">
                    <div class="col-sm-12 text-center">
                        <img
                            :src="'images/' + row.image"
                            alt=""
                            class="img-fluid rounded imgggg"
                        />
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Full Name</label>
                        <input
                            type="text"
                            :value="row.fullname"
                            class="form-control"
                            name="fullname"
                        />
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Email</label>
                        <input
                            type="email"
                            :value="row.email"
                            class="form-control"
                            name="email"
                        />
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Address</label>
                        <textarea
                            name="address"
                            :value="row.address"
                            class="form-control"
                        ></textarea>
                    </div>
                    <div class="col-sm-12 mb-2">
                        <label for="">Image</label>
                        <input
                            @change="loadFile"
                            name="file"
                            class="form-control"
                            type="file"
                            id="file"
                        />
                    </div>
                    <input
                        name="id"
                        type="hidden"
                        class="form-control"
                        :value="row.id"
                        id="contact_id"
                    />
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
    props: ["row"],
    created() {
        console.log(this.$attrs["row"]);
        this.formdata = new FormData();
    },
    data() {
        return {
            data: "",
            formdata: {}
        };
    },
    methods: {
        update: function() {
            let elements = document.getElementsByClassName("form-control");
            let id = document.getElementById("contact_id").value;

            for (let i = 0; i < elements.length; i++) {
                this.formdata.append(
                    elements[i].getAttribute("name"),
                    elements[i].value
                );
            }
            this.formdata.append("_method", "PUT");
            axios
                .post("contact/" + id, this.formdata)
                .then(response => (this.data = response.data))
                .then(data => {
                    this.$emit("updated", 1);
                });

            this.$refs.editForm.reset();
            this.$bvModal.hide("modal-5");
        },
        loadFile: function() {
            let loadFile = document.getElementById("file");
            this.formdata.append("file", loadFile.files[0]);
        }
    }
};
</script>
<style scoped>
.imgggg {
    width: 50%;
    border-radius: 50% !important;
}
</style>
