<template>
    <center>
        <table class="table-auto">
            <tr v-for="item in todos" v-bind:key="item.i" >
                <td v-if="item.status == true"><strike>{{ item.title }}</strike></td>
                <td v-else>{{ item.title }} </td>
                <td>
                    <button v-if="item.status == true" v-on:click="completeFun(item.id, item.status)" class="bg-green-500 hover:bg-green-400 text-white font-bold py-2 px-4 border-b-4 border-green-700 hover:border-green-500 rounded" ref="statusbtn" >Completed</button>{{' '}}
                    <button v-if="item.status == false" v-on:click="completeFun(item.id, item.status)" class="bg-blue-500 hover:bg-blue-400 text-white font-bold py-2 px-4 border-b-4 border-blue-700 hover:border-blue-500 rounded" ref="statusbtn"  >Complete</button>{{' '}}

                    <button v-on:click="deleteFun(item.id)" class="bg-red-500 hover:bg-red-400 text-white font-bold py-2 px-4 border-b-4 border-red-700 hover:border-red-500 rounded">Delete</button>
                </td>
            </tr>
        </table>
    </center>
</template>

<script>
import axios from "axios";

export default {
    name:'TodoList',
    props: { 
        items: String
    },
    data() {
        return {
          todos: []
        };
    },
    async mounted() {
        const res = await axios.get(`http://localhost:8000/api/todos`)
                    .then((res) => {
                        console.log(res.data)
                        this.todos = res.data.reverse()
                    })
                    .catch((err) => {
                    console.log("Data not found")
                    })
    },

    methods: {
        deleteFun(id) {
            axios.delete(`http://localhost:8000/api/todos/${id}`)
                        .then((res) => {
                             console.log("data has deleted")
                             location.reload();
                          })
                        .catch((err) => {
                           console.log("Data not delete")
                        })
        },

        completeFun(id, status) {
            if(status == true)
            {
                status = false;
                console.log(status, 'if')
            }
            else{
                status = true;
                console.log(status, 'else')
            }
            const completeData = {status: status}
            axios.patch(`http://localhost:8000/todos/${id}`, completeData)
                        .then((res) => {
                             console.log("task completed")
                             this.$refs.statusbtn.text="completed"
                             this.$refs.statusbtn.style="green"
                             location.reload();
                          })
                        .catch((err) => {
                           console.log("task is not completed")
                        })
        },

    }
}
</script>