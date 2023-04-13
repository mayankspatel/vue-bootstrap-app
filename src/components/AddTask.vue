<template>
    <form @submit="onSubmit" class="add-form mb-5">
        <div class="mb-3">
            <label class="form-label">Title</label>
            <input type="text" class="form-control" v-model="title" name="title" placeholder="Add Title">
        </div>
        <div class="mb-3">
            <label class="form-label">Description</label>
            <textarea class="form-control" rows="3" v-model="description" name="description" placeholder="Add Description"></textarea>
        </div>
        <div class="mb-3">
            <label class="form-label">Priority</label>
            <select class="form-select" name="priority" v-model="priority">
                <option value="low">Low</option>
                <option value="medium">Medium</option>
                <option value="high">High</option>
            </select>
        </div>
        <div class="mb-3">
            <label class="form-label">Due Date</label>
            <input type="date" class="form-control" v-model="due_date" name="due_date" placeholder="Due Date">
        </div>
        <div class="mb-3 form-check">
            <label class="form-check-label" for="reminder-chk">Set Reminder</label>
            <input type="checkbox" class="form-check-input" v-model="reminder" id="reminder-chk" name="reminder" />
        </div>

        <input type="submit" value="Save Task" class="btn btn-primary">
    </form>
</template>

<script>
export default {
    name: 'AddTask',
    data() {
        return {
            title: '',
            description: '',
            priority: '',
            due_date: '',
            reminder: true
        }
    },
    methods: {
        onSubmit(e) {
            e.preventDefault()
            if(!this.title) {
                alert('Please add a title');
                return 
            } 

            const newTask = {
                // id: Math.floor(Math.random() * 100000),
                title: this.title,
                description: this.description,
                priority: this.priority,
                due_date: this.due_date,
                reminder: this.reminder
            }

            this.$emit('add-task', newTask)
            
            this.title = ''
            this.description = ''
            this.priority = ''
            this.due_date = ''
            this.reminder = false
        }
    }
}
</script>