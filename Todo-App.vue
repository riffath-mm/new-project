<script>
import { ref, reactive } from "vue";
export default {
    name: "HelloWorld",
    props: {
        msg: String,
    },

    setup() {
        // Reactive State using 'ref ' and reactive

        const task = ref(""); // Input task value
        const isEdit = ref(false);
        const taskIndex = ref(null);

        // List of tasks as a reactive object
        const tasks = reactive([
            {
                name: "steal data from ",
                status: "to-do",
            },
            {
                name: "Eat Chocolate ",
                status: "Done",
            },
        ]);

        // Submit Task - ADD OR EDit
        const submitTask = () => {
            if (task.value.length === 0) return;
            if (isEdit.value) {
                // update task
                tasks[taskIndex.value].name = task.value;
                tasks[taskIndex.value].status = "to-do";
                taskIndex.value = null;
                isEdit.value = false;
            } else {
                tasks.push({ name: task.value, status: "to-do" });
            }
            task.value = ""; // Clear input
        };
        const deleteTask = (index) => {
            tasks.splice(index, 1);
        };
        // Edit task
        const editTask = (index) => {
            task.value = tasks[index].name; // Pre-fill task input
            taskIndex.value = index; // Set the task index for editing
            isEdit.value = true; // Set editing flag to true
        };
        return {
            task,
            tasks,
            isEdit,
            taskIndex,
            submitTask,
            deleteTask,
            editTask,
        };
    },
};
</script>

<template>
    <div class="container">
        <h2 class="head">My Vue Todo App</h2>
        <div>
            <input
                v-model="task"
                type="text"
                placeholder="enter"
                class="form-control"
            />
            <button @click="submitTask" class="form-control btn">ADD</button>
        </div>

        <table class="table">
            <thead>
                <tr class="row">
                    <th>TASK</th>
                    <th>Status</th>
                    <th>#</th>
                    <th>#</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index" class="tr">
                    <td>{{ task.name }}</td>
                    <td>{{ task.status }}</td>
                    <td>
                        <span @click="editTask(index)" class="span">Edit</span>
                    </td>
                    <td @click="deleteTask(index)" class="delete">Delete</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<style scoped>
.container {
    text-align: center;
    margin: auto;
    margin-top: 10%;
    padding: 20px;
    width: 350px;
    height: auto;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
    color: #333;
    width: 50%;
    margin-top: 5%;
}

.head {
    font-size: 24px;
    font-weight: bold;
    color: #333;
    margin-bottom: 20px;
}

input.form-control {
    width: 70%;
    padding: 8px;
    font-size: 16px;
    border: 2px solid #ddd;
    border-radius: 4px;
    outline: none;
    transition: border-color 0.3s;
}

input.form-control:focus {
    border-color: #0056b3;
}

button.form-control.btn {
    width: 25%;
    background-color: #007bff;
    color: white;
    padding: 10px;
    font-size: 16px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
}

button.form-control.btn:hover {
    background-color: #0056b3;
}

.table {
    width: 100%;
    margin-top: 20px;
    border-collapse: collapse;
    text-align: left;
}

.table thead {
    background-color: #007bff;
    color: white;
}

.table th,
.table td {
    padding: 12px 15px;
    border-bottom: 1px solid #ddd;
}

.table tbody tr:hover {
    background-color: #f1f1f1;
}

.table td {
    vertical-align: middle;
}

.span {
    color: #007bff;
    cursor: pointer;
    font-weight: bold;
    transition: color 0.3s ease;
}

.span:hover {
    color: #0056b3;
}

.delete {
    color: #ff4d4d;
    cursor: pointer;
    font-weight: bold;
    transition: color 0.3s ease;
}

.delete:hover {
    color: #b30000;
}

@media screen and (max-width: 768px) {
    .container {
        width: 90%;
        margin-top: 5%;
    }

    input.form-control {
        width: 60%;
    }

    button.form-control.btn {
        width: 35%;
    }
}
</style>
