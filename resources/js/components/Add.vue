<template>
<div>


    <div class="modal fade" id="add" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button type="button" class="close" @click="clearModal" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <p class="alert alert-success" v-if="success.length > 0 ">{{ success}}</p>
            <div class="modal-body">
             
                <input type="text" name="name" v-model="name" id="name" placeholder="Title Name" class="form-control" >
                <ul v-if="error.name" class="list-unstyled">
                    <li v-for="err of error.name" class="alwert alert-danger">{{err}}</li>
                </ul>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" @click="clearModal" data-dismiss="modal">Close</button>
                <button type="button" class="btn btn-primary" @click="addRecord">Save changes</button>
            </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    data(){
        return{
            name:'',
            success:'',
            error:[],
        }
    },
    methods:{
        addRecord(){
            axios.post("http://localhost:8080/vuelaravel/tasks",{
                'name': this.name,
            })
            // .then(data => console.log(data))
            .then(data => {
                this.$emit('recordadded', data);//update without refreh browser
                this.success = "Save Data Successfuly!!!";
                this.name=''; //for empty input form

                }) 
             
            // .catch(error => console.log(error))
            // this.name=''; //for empty input form
            .catch(error => {
                this.error = error.response.data.errors;
                console.log(this.error);
            })
           
       
        },
        clearModal(){
            this.error=[];//for error message  filde empty
            this.name=''; //for empty input form

        }
    }

}
</script>
<style scoped>

</style>