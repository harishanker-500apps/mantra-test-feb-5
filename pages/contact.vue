<script setup>
import { compileScript } from 'vue/compiler-sfc';
import formData from '../data/contacts.json';

const showFormModal = ref(false);
const showEditdataModal = ref(false)
const editDataIndex = ref(null)
const editFormData = ref({})
const contactsData = ref([]);

const handleCreateContact = (data) => {
    saveData("contacts data", data);
};

const saveData = (key, data) => {
    const existingData = JSON.parse(localStorage.getItem(key)) || [];
    const newData = [...existingData, data];
    localStorage.setItem(key, JSON.stringify(newData));
    contactsData.value = newData;
    showFormModal.value = false;
};

const editData = (data) =>{
    console.log(data)
    contactsData.value[editDataIndex.value] = {...data};
    editFormData.value = {};
    const newData = [...contactsData.value];
    localStorage.setItem("contacts data", JSON.stringify(newData));
    console.log(contactsData)
    showEditdataModal.value = false; 
}

const handleEditContact = (index, data) => {
    showFormModal.value = false;

    // Create a new object to hold the form data for editing
    const formDataCopy = JSON.parse(JSON.stringify(formData));

    // Iterate through the input fields and set their values from the existing data
    formDataCopy.input.forEach(field => {
        field.value = data[field.name];
    });

    // Set the form data for editing
    editFormData.value = {...formDataCopy};
    // Set the index for editing
    editDataIndex.value = index;

    // Show the form for editing
    showEditdataModal.value = true;
};


const handleDeleteContact = (index) => {
    const newData = [...contactsData.value];
    newData.splice(index, 1);
    localStorage.setItem("contacts data", JSON.stringify(newData));
    contactsData.value = newData;
};

onMounted(() => {
    getData("contacts data");
});


const getData = (key) => {
    const data = JSON.parse(localStorage.getItem(key)) || [];
    contactsData.value = data;
};


const showModalButton = computed(() => {
    if(showEditdataModal.value){
        return 'Hide modal';
    }
    if (!showFormModal.value) {
        return 'Add Contact';
    } else {
        return 'Hide modal';
    }
});

const handleCloseModal = ()=>{
    editFormData.value = {};
    if(showEditdataModal.value || showFormModal.value){
        showEditdataModal.value = false;
        showFormModal.value = false;
        return;
    }
   showFormModal.value = !showFormModal.value;
}
</script>

<template>
    <div class="relative w-screen overflow-auto" style="height: calc(100vh - 40px);">
        <span @click="handleCloseModal" class="fixed top-12 right-3  text-white bg-blue-700 p-2 rounded-md cursor-pointer">{{ showModalButton }}</span>
        <Form v-if="showFormModal" :data="formData" :handleSubmit="handleCreateContact" />
        <Form v-if="showEditdataModal" :data="editFormData" :handleSubmit="editData" />
        <h2 v-if="contactsData.length === 0" class="text-2xl font-semibold mb-4  pl-2 text-center">No Data exist yet ... create new </h2>
        <div v-if="contactsData.length > 0  && !showFormModal && !showEditdataModal" class="mt-8">
            <h2 class="text-2xl font-semibold mb-4 pl-2">Contacts Data</h2>
            <ul>
                <li v-for="(contact, index) in contactsData" :key="index" class="mb-4 border p-4 flex justify-between items-center">
                    <div>
                        <p><strong>Name:</strong> {{ contact.Name }}</p>
                        <p><strong>Phone:</strong> {{ contact.Phone }}</p>
                        <p><strong>Country:</strong> {{ contact.Country }}</p>
                    </div>
                    <div>
                        <button @click="handleEditContact(index,contact)" class="bg-blue-500 text-white px-4 py-2 rounded-md mr-2">Edit</button>
                        <button @click="handleDeleteContact(index)" class="bg-red-500 text-white px-4 py-2 rounded-md">Delete</button>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

