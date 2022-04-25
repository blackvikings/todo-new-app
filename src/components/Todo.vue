<script>
import TodoForm from "./todo-form/TodoForm.vue";
import TodoList from "./list-items/TodoList.vue";

export default {
   name: 'Todo',
   components: { TodoForm, TodoList },
   data() {
      return {
        renderComponent: true,
      };
    },
   methods:{
       forceRerender() {
            // Remove my-component from the DOM
            this.renderComponent = false;

            this.$nextTick(() => {
            // Add the component back in
                this.renderComponent = true;
        });
      },
   }
}

</script>

<template>
    <h1 class="text-5xl text-center font-semibold mb-4 mt-5">Todo App</h1>
    <TodoForm />
    <TodoList />
</template>
