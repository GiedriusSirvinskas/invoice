<template>
    <v-form class="form">
        <v-autocomplete label="Pavadinimas" v-model="selectedName" hide-no-data :items="filteredNames"
            @input="handleNameInput"></v-autocomplete>
        <v-autocomplete label="Įmonės kodas" v-model="selectedCode" hide-no-data :items="filteredCodes"
            @input="handleCodeInput"></v-autocomplete>
        <v-text-field size="20" label="PVM kodas" v-model="formData.pvm"></v-text-field>
        <v-text-field size="20" label="Adresas" v-model="formData.address"></v-text-field>
        <v-text-field size="20" label="El. paštas" v-model="formData.email"></v-text-field>
        <v-text-field size="20" label="Telefonas" v-model="formData.phone"></v-text-field>
        <v-text-field size="20" label="Šalis" v-model="formData.country"></v-text-field>
    </v-form>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            selectedName: "",
            selectedCode: "",
            names: [],
            codes: [],
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
    mounted() {
        axios.get("http://localhost:3000/companies")
            .then(response => this.companies = response.data).catch(error => console.log(error))
    },
    computed: {
        filteredNames() {
            return this.companies.map(company => company.name)
        },
        filteredCodes() {
            return this.companies.map(company => company.code)
        }
    },
    methods: {
        handleNameInput() {
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
        handleCodeInput() {
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