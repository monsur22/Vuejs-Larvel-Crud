<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">

                    <tr v-for="t in tasks.data" :key="t.id">
                       <td>{{ t.id }}-</td>
                      <td>{{ t.name }}</td>
                  

                       <td> 
                           <a @click="getRecord(t.id)" data-toggle="modal" href="#editModal">
                                <i class="material-icons">Edit</i>
                           </a>
                        
                           <button @click="delRecord(t.id)" class="btn btn-danger btn-xs"> Delete </button>
                            <a @click="getRecord(t.id)" data-toggle="modal" href="#viewModal">
                                <i class="material-icons">View</i>
                            </a>
                       </td>

                   </tr>
                   <!-- <pagination :data="tasks" v-on:pagination-change-page="getResults"></pagination> -->
                   <pagination :data="tasks" @pagination-change-page="getResults"></pagination>
                </div>
            </div>
        </div>
        <!-- <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#add">Add</button> -->
        <a class="btn btn-primary" data-toggle="modal" href="#add">Add</a>
    <div id="modal">
    <Add @recordadded="refreshRecord"></Add>
    <Edit :rec="editRec" @recordUpdate="refreshRecord"></Edit>
    <Viewdata :viewRec="editRec"></Viewdata>

    </div>

    </div>

</template>

<script>

export default {
    name: 'Home',
    data(){
        return{
            tasks:{},
            name:{},
            editRec:{},
            errors:[],
        }
    },
    methods:{
		getResults(page = 1) {
			axios.get('http://localhost:8080/vuelaravel/tasks?page=' + page)
			.then(response =>{this.tasks = response.data})
            .catch(error => console.log(error.response));
        },
        getRecord(id){
            console.log(id);
            axios.get('http://localhost:8080/vuelaravel/tasks/' +id+'/edit')
			.then(response =>{this.editRec = response.data})
			// .then(response =>console.log(response.data)) //for show in console 
            .catch(error => this.errors = error.response.data.errors);
        },
        delRecord(id){
            const reply = confirm("sure");
            if(reply){
                axios.post("http://localhost:8080/vuelaravel/tasks/"+id,{
                'id': id,
                '_method':'DELETE'
                })
                .then(response => this.refreshRecord(response))
                .catch(error => console.log(error.response));
            }else{
                return 

            }

        },
         refreshRecord(name){
            this.tasks = name.data

        }
  


    },
       
        created(){

            axios.get("http://localhost:8080/vuelaravel/tasks")
            .then(response =>{this.tasks = response.data})
            .catch(error => console.log(error.response));
        }
    // mounted(){
        
    // }

}
</script>
<style scoped>

</style>
