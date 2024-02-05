<script setup>
import formData from '../data/contacts.json';

const showFormModal = ref(false);
const showEditdataModal = ref(false)
const editDataIndex = ref(null)
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
    const newData = [...contactsData.value];
    localStorage.setItem("contacts data", JSON.stringify(newData));
    console.log(contactsData)
    showEditdataModal.value = false; 
}

const handleEditContact = (index) => {
    editDataIndex.value = index;
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
    console.log(data);
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
    if(showEditdataModal.value){
        showEditdataModal.value = false;
        return;
    }
   showFormModal.value = !showFormModal.value;
}
</script>

<template>
    <div class="relative w-screen min-h-[100vh]">
        <span @click="handleCloseModal" class="fixed top-12 right-3 bg-blue-700 p-2 rounded-md cursor-pointer">{{ showModalButton }}</span>
        <Form v-if="showFormModal" :data="formData" :handleSubmit="handleCreateContact" />
        <Form v-if="showEditdataModal" :data="formData" :handleSubmit="editData" />
        
        <div v-if="contactsData.length > 0" class="mt-8">
            <h2 class="text-2xl font-semibold mb-4 pl-2">Contacts Data</h2>
            <ul>
                <li v-for="(contact, index) in contactsData" :key="index" class="mb-4 border p-4 flex justify-between items-center">
                    <div>
                        <p><strong>Name:</strong> {{ contact.Name }}</p>
                        <p><strong>Phone:</strong> {{ contact.Phone }}</p>
                        <p><strong>Country:</strong> {{ contact.Country }}</p>
                    </div>
                    <div>
                        <button @click="handleEditContact(index)" class="bg-blue-500 text-white px-4 py-2 rounded-md mr-2">Edit</button>
                        <button @click="handleDeleteContact(index)" class="bg-red-500 text-white px-4 py-2 rounded-md">Delete</button>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>
