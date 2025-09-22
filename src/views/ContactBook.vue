<script>
import ContactCard from '@/components/ContactCard.vue';
import ContactList from '@/components/ContactList.vue';
import InputSearch from '@/components/InputSearch.vue';
import contactService from '@/services/contact.service';
export default {
    components: {
        InputSearch,
        ContactList,
        ContactCard,
    },
    data() {
        return {
            contacts: [],
            activeIndex: -1,
            searchText: "",
        };
    },
    watch: {
        searchTex() {
            this.activeContact = -1;
        },
    },
    computed: {
        contactString() {
            return this.contacts.map((contact) => {
                const { name, email, address, phone } = contact;
                return [name, email, address, phone].join("");
            });
        },
        filteredContacts() {
            if (!this.searchText) return this.contacts;

            const text = this.searchText.toLowerCase();
            return this.contacts.filter((contact) => {
                return (
                    contact.name?.toLowerCase().includes(text) ||
                    contact.email?.toLowerCase().includes(text) ||
                    contact.phone?.toLowerCase().includes(text)
                );
            });
        },

        filteredContactsCount() {
            return this.filteredContacts.length;
        },

        activeContact() {
            if (this.activeIndex < 0) return null;
            return this.filteredContacts[this.activeIndex];
        }
    },
    methods: {
        async retrieveContacts() {
            try {
                this.contacts = await contactService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        refreshList() {
            this.retrieveContacts();
            this.activeIndex = -1;
        },
        async removeAllContacts() {
            if (confirm("Bạn muốn xóa tất cả liên hệ?")) {
                try {
                    await contactService.deleteAll();
                    this.refreshList();
                } catch (error) {
                    console.log(error);
                }
            }
        },
        goToAddContact() {
            this.$router.push({ name: "contact.add" });
        },
    },
    mounted() {
        this.refreshList();
    }
};
</script>
<style>
.page {
    text-align: left;
    max-width: 750px;
}
</style>

<template>
    <div class="page container">
        <div class="row mb-3">
            <div class="col-12">
                <InputSearch v-model="searchText" />
            </div>
        </div>

        <div class="row">
            <div class="col-md-6">
                <h4>
                    Danh bạ <i class="fas fa-address-book"></i>
                </h4>

                <ContactList v-if="filteredContactsCount > 0" :contacts="filteredContacts"
                    v-model:activeIndex="activeIndex" />
                <p v-else>Không có liên hệ nào.</p>

                <div class="mt-3 d-flex justify-content-between">
                    <button class="btn btn-sm btn-info" @click="refreshList">
                        <i class="fas fa-sync-alt"></i> Làm mới
                    </button>

                    <button class="btn btn-sm btn-success" @click="goToAddContact">
                        <i class="fas fa-plus"></i> Thêm mới
                    </button>

                    <button class="btn btn-sm btn-danger" @click="removeAllContacts">
                        <i class="fas fa-trash"></i> Xóa tất cả
                    </button>
                </div>
            </div>
            <div class="mt-3 col-md-6">
                <div v-if="activeContact">
                    <h4>
                        Chi tiết liên hệ
                        <i class="fas fa-address-card"></i>
                    </h4>
                    <ContactCard :contact="activeContact" />
                </div>
            </div>
        </div>
    </div>
</template>




<!-- <template>
    <div class="page row">
        <div class="col-md-10">
            <InputSearch v-model="searchText" />
        </div>

        <div class="mt-3 col-md-6">
            <h4>
                Danh bạ
                <i class="fas fa-address-book"></i>
            </h4>

            <ContactList v-if="filteredContactsCount > 0" :contacts="filteredContacts"
                v-model:activeIndex="activeIndex" />
            <p v-else>Không có liên hệ nào.</p>
            <div class="mt-3 row jusify-content-around align-items-center">
                <button class="btn btn-sm btn-primary" @click="refreshList()">
                    <i class="fas fa-redo"></i> Làm mới
                </button>

                <button class="btn btn-sm btn-success" @click="goToAddContact">
                    <i class="fas fa-plus"></i> Thêm mới
                </button>

                <button class="btn btn-sm btn-danger" @click="removeAllContacts">
                    <i class="fas fa-trash"></i> Xóa tất cả
                </button>
            </div>
        </div>
    </div>
</template> -->