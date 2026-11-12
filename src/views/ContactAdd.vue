<template>
    <div class="page">
        <h4>Thêm Liên hệ</h4>
        <ContactForm
            :contact="contact"
            :show-cancel="false"
            @submit:contact="addContact"
        />
        <p v-if="errorMessage" class="error-feedback">{{ errorMessage }}</p>
    </div>
</template>

<script>
import ContactForm from "@/components/ContactForm.vue";
import ContactService from "@/services/contact.service";

export default {
    components: {
        ContactForm,
    },
    data() {
        return {
            contact: {
                name: "",
                email: "",
                address: "",
                phone: "",
                favorite: false,
            },
            errorMessage: "",
        };
    },
    methods: {
        async addContact(contact) {
            this.errorMessage = "";
            try {
                await ContactService.create(contact);
                await this.$router.push({ name: "ContactBook" });
            } catch (error) {
                this.errorMessage =
                    error.response?.data?.message || "Không thể thêm liên hệ.";
            }
        },
    },
};
</script>

<style scoped>
.page {
    max-width: 750px;
    margin: 1rem auto;
}
</style>
