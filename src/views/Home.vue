<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <form @submit="onSubmit" v-show="showEditTask" class="edit-form mb-5">
        <div class="mb-3">
            <label class="form-label">Title</label>
            <input type="text" class="form-control" v-model="details.title" name="title" placeholder="Edit Title">
        </div>
        <div class="mb-3">
            <label class="form-label">Description</label>
            <textarea class="form-control" rows="3" v-model="details.description" name="description" placeholder="Edit Description"></textarea>
        </div>
        <div class="mb-3">
            <label class="form-label">Priority</label>
            <select class="form-select" name="priority" v-model="details.priority">
                <option value="low">Low</option>
                <option value="medium">Medium</option>
                <option value="high">High</option>
            </select>
        </div>
        <div class="mb-3">
            <label class="form-label">Due Date</label>
            <input type="date" class="form-control" v-model="details.due_date" name="due_date" placeholder="Edit Due Date">
        </div>
        <div class="mb-3 form-check">
            <label class="form-check-label" for="reminder-chk">Set Reminder</label>
            <input type="checkbox" class="form-check-input" v-model="details.reminder" id="reminder-chk" name="reminder" />
        </div>
        <input type="submit" value="Update Task" class="btn btn-primary">
    </form>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" @edit-task="editTask" :tasks="tasks" />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
    name: 'Home',
    props: {
        showAddTask: Boolean
    },
    components: {
        Tasks,
        AddTask
    },
    data() {
        return {
            tasks: [],
            showEditTask: false,
            details: {
                title : '',
                description : '',
                priority : '',
                due_date : '',
                reminder : true
            } 
        }
    },
    methods: {
        async onSubmit(e) {
            e.preventDefault()
            const newTask = {
                id: this.details.id,
                title: this.details.title,
                description: this.details.description,
                priority: this.details.priority,
                due_date: this.details.due_date,
                reminder: this.details.reminder
            }

            var id = this.details.id

            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(newTask)
            })

            const data = await res.json() 
            
            this.tasks = this.tasks.map((task) => task.id === id ? { ...task, title: data.title, description: data.description, priority: data.priority, due_date: data.due_date, reminder: data.reminder } : task)

            this.showEditTask = !this.showEditTask
        },
        async editTask(id) {
            const editData = await this.fetchTask(id)
            console.log(editData)
            this.details = {
                id: editData.id,
                title: editData.title,
                description: editData.description,
                priority: editData.priority,
                due_date: editData.due_date,
                reminder: editData.reminder
            }
            this.showEditTask = true
            this.scrollToTop()  
        },
        async addTask(task) {
            const res = await fetch('http://localhost:5000/tasks', {
                method: 'POST',
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(task)
            })

            const data = await res.json()
            this.tasks = [...this.tasks, data]
        },
        async deleteTask(id) {
            if(confirm('Are you sure?')) {
                const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                    method: 'DELETE'
                })

                res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')  
            }
        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id)
            const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

            const res = await fetch(`http://localhost:5000/tasks/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-type': 'application/json'
                },
                body: JSON.stringify(updTask)
            })

            const data = await res.json() 

            this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
        },
        async fetchTasks() {
            const res = await fetch('http://localhost:5000/tasks');
            const data = res.json()
            return data
        },
        async fetchTask(id) {
            const res = await fetch(`http://localhost:5000/tasks/${id}`);
            const data = res.json()
            return data
        },
        scrollToTop() {
            window.scrollTo({ top: 0, behavior: "smooth" });
        }
    },
    async created() {
        this.tasks = await this.fetchTasks()
    }   
}
</script>