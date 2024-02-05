<script setup>
import formData from '../data/emloyeeForm.json';

const showFormModal = ref(false);
const showEditdataModal = ref(false)
const editDataIndex = ref(null)
const employeeData = ref([]);

const handleCreateEmployee = (data) => {
    saveData("employee data", data);
};

const saveData = (key, data) => {
    const existingData = JSON.parse(localStorage.getItem(key)) || [];
    const newData = [...existingData, data];
    localStorage.setItem(key, JSON.stringify(newData));
    employeeData.value = newData;
    showFormModal.value = false;
};

const editData = (data) =>{
    console.log(data)
    employeeData.value[editDataIndex.value] = {...data};
    const newData = [...employeeData.value];
    localStorage.setItem("emloyee data", JSON.stringify(newData));
    console.log(employeeData)
    showEditdataModal.value = false;    
}

const handleEditEmployee = (index) => {
    editDataIndex.value = index;
    showEditdataModal.value = true;
};  


const handleDeleteEmployee = (index) => {
    const newData = [...employeeData.value];
    newData.splice(index, 1);
    localStorage.setItem("employee data", JSON.stringify(newData));
    employeeData.value = newData;
};

onMounted(() => {
    getData("employee data");
});

const getData = (key) => {
    const data = JSON.parse(localStorage.getItem(key)) || [];
    console.log(data);
    employeeData.value = data;
};

const showModalButton = computed(() => {
    if(showEditdataModal.value){
        return 'Hide modal';
    }
    if (!showFormModal.value) {
        return 'Add Employee';
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
        <Form v-if="showFormModal" :data="formData" :handleSubmit="handleCreateEmployee" />
        <Form v-if="showEditdataModal" :data="formData" :handleSubmit="editData" />
        
        <div v-if="employeeData.length > 0" class="mt-8">
            <h2 class="text-2xl font-semibold mb-4  pl-2">Employee Data</h2>
            <ul>
                <li v-for="(employee, index) in employeeData" :key="index" class="mb-4 border p-4 flex justify-between items-center">
                    <div>
                        <p><strong>Name:</strong> {{ employee.Name }}</p>
                        <p><strong>Age:</strong> {{ employee.Age }}</p>
                        <p><strong>Gender:</strong> {{ employee.Gender }}</p>
                        <p><strong>Date Of Joining:</strong> {{ employee['Date Of Joining'] }}</p>
                        <p><strong>Designation:</strong> {{ employee.Designation }}</p>
                    </div>
                    <div>
                        <button @click="handleEditEmployee(index)" class="bg-blue-500 text-white px-4 py-2 rounded-md mr-2">Edit</button>
                        <button @click="handleDeleteEmployee(index)" class="bg-red-500 text-white px-4 py-2 rounded-md">Delete</button>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>
