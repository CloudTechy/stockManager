<template>
    <div id="example1_wrapper" class="dataTables_wrapper  container-fluid dt-bootstrap4">
        <div class="row">
            <div class="col-6 col-md-6">
                <div class="dataTables_length" id="example1_length">
                    <label>Entries:
                        <select v-model = "rowsPerPage" @change = "loadUnits" aria-controls="dataTables-example" class="form-control input-sm"> 
                            <option value="5">5</option>
                            <option value="10">10</option> 
                            <option value="25">25</option>
                            <option value="50">50</option>
                            <option value="100">100</option>
                        </select> 
                    </label>
                </div>
            </div>
            <div class="col-6 col-md-6">
                <div id="example1_filter" class="dataTables_filter float-right">
                    <label>Search:<input v-model="search" type="search" class="form-control form-control-sm" placeholder="search unit" aria-controls="example1">
                    </label>
                </div>
            </div> 
        </div>
        <div class="row">
            <div class="col-sm-12" >
                <div class="table-responsive-sm">
                <table  class="table  table-hover dataTable" >
                    <thead>
                        <tr role="row " >
                            <th>Unit Name</th>
                            <th>Status</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody id="body">        
                        
                        <tr v-for = "unit,index in pageLoader(current_page)">
                            <td class="text-capitalize">{{  unit.name }}</td>
                            <td><span class="badge badge-success">active</span></td>
                            <td>
                                <button @click="loadEditUnit(unit,index)" type="button" title="edit this unit" class="btn btn-outline-primary m-1"  data-toggle="modal" data-target="#editUnitModal" ><i class="fas fa-pen"></i></button>
                                <button @click="deleteUnit(unit.name,index+start)" type="button" title="delete this unit" class="btn btn-outline-danger m-1"><i class="fas fa-trash-alt"></i></button>
                            </td>
                            
                        </tr>
                         <tr v-if = "loading == false && pageLoader(current_page).length == 0">
                            <td colspan="4">
                                <h4 class="text-center">Unit Not Found</h4>
                            </td>
                        </tr>
                     
                    </tbody>
                </table>
                </div>
            </div>
        </div>

        <div class="row">
            <div class="col-6 col-md-6">
                <div class="dataTables_info" id="example1_info" role="status" aria-live="polite">Showing {{ start + 1 }} to {{ end > length ? length : end }} of {{ length }} entries
                </div>
            </div>
            <div class="col-6 col-md-6">
                <div class="dataTables_paginate paging_simple_numbers float-right" id="example1_paginate">
                    <ul class="pagination">
                        <li class="paginate_button page-item previous" ref = "prev" id="example1_previous">
                            <a href="#" @click.prevent ="pageLoaderB(-1)" aria-controls="example1" data-dt-idx="0" tabindex="0" class="page-link">Previous
                            </a>
                        </li>
                        <li class="paginate_button page-item" style="display: inline"  v-if = "Math.floor(pages) > 6">
                            <div class="m-auto p-2 bg-primary paginate_button page-item">
                                <a  @click.prevent = "true" href="#">{{current_page }}</a>
                                <a  @click.prevent = "true" href="#">of</a>    
                                <a  @click.prevent = "true" href="#">{{Math.floor(pages) }}</a>
                            </div>
                        </li>
                        <li v-else v-for = "value in Math.floor(pages)" v-bind:class="classObject(value)" class="paginate_button page-item" >
                            <a  aria-controls="example1" data-dt-idx="1" tabindex="0" class="page-link" @click.prevent ="pageLoader(value)" href="#">{{ value }}</a>
                        </li>
                       
                        <li class="paginate_button page-item next" ref = "next" id="example1_next">
                            <a @click.prevent ="pageLoaderB(1)" href="#" aria-controls="example1" data-dt-idx="7" tabindex="0" class="page-link">Next
                            </a>
                        </li>
                    </ul>
                </div>
            </div>
        </div>

        <div class="modal fade" id="editUnitModal">
        <edit-unit-component></edit-unit-component>
        </div>

    </div>
</template>

<script>
    
    export default {
        mounted() {
<<<<<<< HEAD
            if(this.$cookies.get('units')){
                this.units = JSON.parse(this.$cookies.get('units'))
            }
=======
            
>>>>>>> a90f05ca68e2264c685a9477281ef51e4d16983b
        },

        data() { 
            var d = new Date();
            return {
                month : d.getMonth() + 1,
                year : d.getFullYear(),
                units : [],
                loading : true,
                error : '',
                search : '',
                rowsPerPage: 5,
                current_page: 1,
                length: 0,
                pages : 0,
                form: new Form()
            }

        },
        watch : {
            
            filteredUnits: function(){
                this.loading = false;
            },
           
            
        },
        created(){
<<<<<<< HEAD
            if(!this.$cookies.get('units')){
                this.loadUnits();
            }
            Fire.$on('unit_created', (data)=> {
                this.loadUnits();
=======
            Fire.$on('unit_created', (data)=> {
                this.loadUnits();

>>>>>>> a90f05ca68e2264c685a9477281ef51e4d16983b
            })
            Fire.$on('unit_deleted', (data)=> {
                this.loadUnits();
            })
            Fire.$on('unit_edited', (data)=> {
                this.loadUnits();
            })
<<<<<<< HEAD
            Echo.channel('unit')
            .listen('UpdateUnit', (e) => {
                this.loadUnits();
            });
=======
            this.loadUnits();
>>>>>>> a90f05ca68e2264c685a9477281ef51e4d16983b
        },

        computed: {
            filteredUnits (){
                var data = [];
              if(this.search){
              data =  this.units.filter((item)=>{
                return item.name.toLowerCase().includes(this.search.toLowerCase());
              })
              }else{
              data = this.units;
              }
              this.length = data.length;
              this.pages =  Math.ceil(data.length / this.rowsPerPage);
              return data;
            },
            start(){
                if (this.pages > 0  && this.current_page  >=  this.pages ) {
                    this.current_page = this.pages
                }
                return parseInt(this.rowsPerPage * (this.current_page -1));
            },
            end(){
                return parseInt(this.rowsPerPage * (this.current_page - 1)) + parseInt(this.rowsPerPage);  
            }         
        },
        methods: {
            loadUnits(){
                this.$Progress.start();
                var form = new Form()
                form.get('./api/units?pageSize=1000000')
                .then( response => {
                    if(response.data.status == true){
                        this.$Progress.finish()
                        Fire.$emit('units_loaded', response.data.data)
                        this.units = response.data.data.item.length !=0 ? response.data.data.item : [];
<<<<<<< HEAD
                        this.$cookies.set('units',JSON.stringify(this.units))
=======
>>>>>>> a90f05ca68e2264c685a9477281ef51e4d16983b
                    }
                    else{
                        this.$Progress.fail()
                        this.$root.alert('error','error','An unexpected error occured, Try again Later')
                    }
                })
                .catch(error=> {
                    this.$Progress.fail()
                    this.$root.alert('error','error',error.response.data.message)
                    console.log(error.response.data.error)
                }); 
            },
            classObject (value) {
                if(value <= 1 && this.current_page == 1){
                    this.$refs.prev.classList.add('disabled')
                }
                else if(value <= 1 && this.current_page > 1){
                    this.$refs.prev.classList.remove('disabled')
                }
                if(this.current_page == this.pages){
                    this.$refs.next.classList.add('disabled')
                }
                else{
                  this.$refs.next.classList.remove('disabled')   
                }
                if (value == this.current_page) {
                    return "active";
                }
            },
            pageLoader(pageNumber){
                if(this.pages > 6){
                    this.$refs.prev.classList.remove('disabled')
                    this.$refs.next.classList.remove('disabled')
                }
                this.current_page = pageNumber;
               return this.filteredUnits.slice(this.start,this.end);
            },
            pageLoaderB(amount){
                if(this.current_page <= 1 && amount == -1){
                   this.current_page = 1; 
                }
                else if(this.current_page >= this.page){
                    this.current_page = this.page;
                }
                else{
                   this.current_page += amount;
                }
            },
            loadEditUnit(unit,index){
                Fire.$emit('edit_unit',unit);
            },
            deleteUnit(name,index){
            this.$swal({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                })
                .then((result) => {
                    if (result.value) {
                        this.$Progress.start();
                        this.form.delete('./api/units/'+name)
                        .then(response => {
                            if(response.data.status == true){
                                Fire.$emit('unit_deleted', response.data.data)
                                this.$Progress.finish()
                                this.$root.alert('success','success','unit deleted')
                            }
                            else{
                                this.$Progress.fail()
                                this.$root.alert('error','error','An unexpected error occured, Try again Later')
                            }
                        })
                        .catch( error => {
                            this.$Progress.fail()
                            this.$root.alert('error','error',error.response.data.message)
                            console.log(error.response.data.error)
                        })
                    }
                }) 
            },
        }
    }

</script>