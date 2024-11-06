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
    border: 2px solid black;
    margin: auto;
    margin-top: 15%;
    width: 30%;
    height: 100%;
    background-color: blanchedalmond;
    color: rgb(109, 101, 101);
}

.table {
    margin: auto;
    margin-top: 20px;
    cursor: pointer;

    /* text-align: center; */
}

.span {
    color: blue;
}
.form-control {
    height: 25px;
}
.delete {
    color: red;
}

.btn {
    background: blue;
    color: white;
    height: 30px;
}
</style>
