<template>

    <div class="modal-dialog  modal-dialog-scrollable">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title">Edit Brand</h4>
            <button type="button" class="close" data-dismiss="modal">&times;</button>
          </div>
          <div class="modal-body">
                
                 <form role="form" ref="form" @keydown="form.onKeydown($event)" @submit.prevent='editBrandAxios' >
                    <div class="card-body">
                        <div class="form-group">
                            <label for="brand_name" class="text-capitalize">Brand Name</label>
                            <input type="text" v-model="form.type" required="" class="form-control" ref="brand_name" placeholder="Enter brand name">
                            <has-error :form="form" field="type"></has-error>
                        </div>
                        <div class="form-group">
                            <label for="brand_description" class="text-capitalize">Brand Description</label>
                            <input v-model = "form.description" type="text" class="form-control" ref="brand_description" placeholder="Enter brand description">
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
        created(){
            Fire.$on('edit_brand', (data)=> {this.form.fill(data); this.brand = data})
        },
        data() { 
            return {
               	form : new Form({
                    type: '',
                    description:'No Description Yet',
                    id:''
                }),
                brand: ''
            }
        },
        beforeDestroy(){
            this.$refs.closeButton.click()
            this.form.reset()
        },
        methods: {
            
            editBrandAxios(){
                this.$Progress.start();
                    this.form.patch('./api/attributes/'+this.brand.id)
                    .then(response => {
                        this.$refs.closeButton.click()

                        if(response.data.status == true){
                            Fire.$emit('brand_edited')
                            this.form.reset()
                            this.$refs.brand_name.classList.remove('is-invalid')
                            this.$refs.brand_description.classList.remove('is-invalid');
                            this.$Progress.finish()
                            this.$root.alert('success','success','brand edited')
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
                        if(error.type){
                            this.$refs.brand_name.classList.add('is-invalid');
                        }
                        if (error.description) {
                            this.$refs.brand_description.classList.add('is-invalid');
                        }
                }); 
            },

        }

    }

</script>