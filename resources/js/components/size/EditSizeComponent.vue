 class="text-capitalize"<template>
    
    <div class="modal-dialog  modal-dialog-scrollable">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title">Edit Size</h4>
            <button type="button" class="close" data-dismiss="modal">&times;</button>
          </div>
          <div class="modal-body">
                 <form role="form" ref="form" @keydown="form.onKeydown($event)" @submit.prevent='editSizeAxios' >
                    <div class="card-body">
                        <div class="form-group">
                            <label for="size_name" class="text-capitalize">Size Name</label>
                            <input type="text" v-model="form.name" required="" class="form-control" ref="size_name" placeholder="Enter size name">
                            <has-error :form="form" field="name"></has-error>
                        </div>
                        <div class="form-group">
                            <label for="size_description" class="text-capitalize">Size Description</label>
                            <input v-model = "form.description" type="text" class="form-control" ref="size_description" placeholder="Enter size description">
                            <has-error :form="form" field="description"></has-error>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" ref = "closeButton" class="btn btn-danger" data-dismiss="modal">Close</button>
                        <button type="submit":disabled="form.busy" class="btn btn-primary">Save changes</button>
                    </div>
                </form>
            </div>
        </div>
    </div>
      
</template>

<script>
    import { 
      HasError,
      AlertError,
      AlertErrors, 
      AlertSuccess
    } from 'vform'
     
    Vue.component(HasError.name, HasError)
    Vue.component(AlertError.name, AlertError)
    Vue.component(AlertErrors.name, AlertErrors)
    Vue.component(AlertSuccess.name, AlertSuccess)
    
    export default {

        data() { 
            return {
                form : new Form({
                    name: '',
                    description:'No Description Yet',
                }),
                size:'',
            }
        },
        created(){
            Fire.$on('edit_size', (data)=> {this.form.fill(data); this.size = data})
        },
        beforeDestroy(){
            this.$refs.closeButton.click();
            this.form.reset();
        },
        methods: {
            editSizeAxios(){
            	this.$Progress.start();
                this.form.patch('./api/sizes/'+this.size.name)
                .then(response => {
                    this.$refs.closeButton.click()
                    if(response.data.status == true){
                        Fire.$emit('size_edited')
                        this.form.reset()
                        this.$refs.size_name.classList.remove('is-invalid')
                        this.$refs.size_description.classList.remove('is-invalid');
                        this.$Progress.finish()
                        this.$root.alert('success','success','size edited')
                    }

                    else{
                        this.$Progress.fail()
                        this.$root.alert('error','error','An unexpected error occured, Try again Later')
                    }
                })
                .catch( error => {
                    this.$Progress.fail()
                    this.$root.alert('error','error',error.response.data.message)
                    var error = error.response.data.error;
                    console.log(error);
                    if(error.name){
                        this.$refs.size_name.classList.add('is-invalid');
                    }
                    if (error.description) {
                        this.$refs.size_description.classList.add('is-invalid');
                    }
                });  
            },

        }

    }

</script>