<template>
<div>


    <div class="modal fade" id="editModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Edit</h5>
                <button type="button" class="close" @click="clearModal" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <p class="alert alert-success" v-if="success.length > 0 ">{{ success}}</p>
            <div class="modal-body">
             
                <input type="text" name="name" v-model="rec.name" id="name" placeholder="Title Name" class="form-control" >
                <ul v-if="error.name" class="list-unstyled">
                    <li v-for="err of error.name" class="alwert alert-danger">{{err}}</li>
                </ul>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" @click="clearModal" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary" @click="updateRecord">Update Record</button>
            </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    props:['rec'],
    data(){
        return{
            name:'',
            success:'',
            error:[],
        }
    },
    methods:{
        updateRecord(){
            axios.post("http://localhost:8080/vuelaravel/tasks/"+this.rec.id,{
                'name': this.rec.name,
                '_method':'PUT'
            })
            // .then(data => console.log(data))
            .then(data => {
                this.$emit('recordUpdate', data);//update without refreh browser
                this.success = "Update Data Successfuly!!!";
                // this.name=''; //for empty input form

                }) 
             
   
            .catch(error => {
                this.error = error.response.data.errors;
                console.log(this.error);
            })
           
       
        },
        clearModal(){
            this.error=[];//for error message  filde empty
            // this.name=''; //for empty input form
            this.success=''; //for empty input form

        }
    }

}
</script>
<style scoped>

</style>