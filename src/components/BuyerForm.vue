<template>
    <v-form class="form">
        <v-text-field label="Pavadinimas" v-model="selectedName" hide-no-data hide-details
            :items="filteredNames" @input="handleNameInput" @focus="nameModal = true"></v-text-field>
        <div v-if="filteredNames && nameModal">
            <ul class="list">
                <li v-for="name in filteredNames" @click="setName(name)" :key="name">{{ name }}</li>
            </ul>
        </div>
        <v-text-field label="Įmonės kodas" v-model="selectedCode" hide-no-data hide-details :items="filteredCodes"
            @input="handleCodeInput" @focus="codeModal = true"></v-text-field>
        <div v-if="filteredCodes && codeModal">
            <ul class="list">
                <li v-for="code in filteredCodes" @click="setCode(code)" :key="code">{{ code }}</li>
            </ul>
        </div>
        <v-text-field size="20" label="PVM kodas" v-model="formData.pvm" hide-details></v-text-field>
        <v-text-field size="20" label="Adresas" v-model="formData.address" hide-details></v-text-field>
        <v-text-field size="20" label="El. paštas" v-model="formData.email" hide-details></v-text-field>
        <v-text-field size="20" label="Telefonas" v-model="formData.phone" hide-details></v-text-field>
        <v-text-field size="20" label="Šalis" v-model="formData.country" hide-details></v-text-field>
    </v-form>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            nameModal: false,
            codeModal: false,
            selectedName: "",
            selectedCode: "",
            companies: [],
            formData: {
                name: "",
                code: "",
                pvm: "",
                address: "",
                email: "",
                phone: "",
                country: ""
            }
        }
    },
    // mounted() {
    //     axios.get("http://localhost:3000/companies").then(response => this.companies = response.data)
    // },
    computed: {
        filteredNames() {
            return this.companies.map(company => company.name)
        },
        filteredCodes() {
            return this.companies.map(company => company.code)
        }
    },
    methods: {
        setName(name) {
            this.selectedName = name;
            this.nameModal = false;
        },
        setCode(code) {
            this.selectedCode = code;
            this.codeModal = false;
        },
        async handleNameInput() {
            await axios.get(`http://localhost:3000/companies?q=${this.selectedName}`).then(response => this.companies = response.data)
            const selectedCompany = this.companies.find(company => company.name === this.selectedName);

            if (selectedCompany) {
                this.selectedName = selectedCompany.name;
                this.selectedCode = selectedCompany.code;
                this.formData.pvm = selectedCompany.pvm;
                this.formData.address = selectedCompany.address;
                this.formData.email = selectedCompany.email;
                this.formData.phone = selectedCompany.phone;
                this.formData.country = selectedCompany.country;
            }

        },
        async handleCodeInput() {
            await axios.get(`http://localhost:3000/companies?q=${this.selectedCode}`).then(response => this.companies = response.data)
            const selectedCompany = this.companies.find(company => company.code === this.selectedCode);

            if (selectedCompany) {
                this.selectedName = selectedCompany.name;
                this.selectedCode = selectedCompany.code;
                this.formData.pvm = selectedCompany.pvm;
                this.formData.address = selectedCompany.address;
                this.formData.email = selectedCompany.email;
                this.formData.phone = selectedCompany.phone;
                this.formData.country = selectedCompany.country;
            }

        },
    },
    watch: {
        selectedName(newVal) {
            this.handleNameInput();
        },
        selectedCode(newVal) {
            this.handleCodeInput();
        }
    }

}

</script>

<style>
.list {
    list-style-type: none;
    z-index: 999;
}

.list li {
    padding: 10px;
    padding-left: 16px;
}
</style>