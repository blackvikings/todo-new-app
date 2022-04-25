<template>
    <form>
        <label class="block">
            <div class="md:container md:mx-auto">
                <div class="grid grid-flow-col md:grid-flow-row">
                    <input v-model="task" class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400
                                                focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500
                                                disabled:bg-slate-50 disabled:text-slate-500 disabled:border-slate-200 disabled:shadow-none
                                                invalid:border-pink-500 invalid:text-pink-600
                                                focus:invalid:border-pink-500 focus:invalid:ring-pink-500" type="text" ref="task">
                    
                    <button type="button" class="mt-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" v-on:click="submitHandle">Submit</button>
                </div>
            </div>
        </label>
    </form>
</template>

<script>

import axios from "axios";
import { ref } from '@vue/reactivity';

let task = ref('');

export default {
    name : 'TodoForm',
    data() {
        return {
          todos: []
        }
    },
    methods: {
        submitHandle() {
          let taskValue = this.$refs.task.value;
          if(taskValue == '' || taskValue == undefined)
          {
            this.$refs.task.focus()
          }
          else {
                  const data = {title: this.task, status: false}
                  axios.post(`http://localhost:8000/api/todos`, data)
                  .then((res) => {
                    console.log("data inserted")
                    location.reload();
                  })
                  .catch((err) => {
                    console.log("Not inserted")
                  })
             this.$refs.task.value=null

          }
            
        }
    },
}
</script>

<style>
  .btn {
    @apply font-bold py-2 px-4 rounded;
  }
  .btn-blue {
    @apply bg-blue-500 text-white;
  }
  .btn-blue:hover {
    @apply bg-blue-700;
  }
</style>