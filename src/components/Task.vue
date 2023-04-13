<template>
    <div class="row d-flex flex-nowrap">
        <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder border-start border-5 border-warning' : '', 'task']">
            <div class="text-icons">
                <h5>{{ task.title }} <span class="text-capitalize">({{ task.priority }})</span></h5>
            </div>
            <p>{{ task.description.length < 150 ? task.description : task.description.substring(0,150) + "..." }}</p>
            <small class="text-muted">Due Date : {{ task.due_date }}</small>
        </div>
        <div class="icons w-25 d-flex align-items-center justify-content-center bg-light">
            <button @click="$emit('delete-task')" title="Delete" class="btn btn-danger mx-1"><i class="fas fa-trash"></i></button>
            <button @click="$emit('edit-task')" title="Edit" class="btn btn-primary"><i class="fas fa-pencil"></i></button>
        </div>
    </div>
    
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object,
    }
}
</script>

<style>
.task {
    background: #f4f4f4;
    margin: 5px;
    padding: 10px 20px;
    cursor: pointer;
}
</style> 