<template>
  <div>

	<nav class="panel">
  <p class="panel-heading">
    phone book
    <button class="button is-link is-outlined" @click="openAdd">
      add new
    </button>
  </p>
  <div class="panel-block">
    <p class="control has-icons-left">
      <input class="input is-small" type="text" placeholder="search">
      <span class="icon is-small is-left">
        <i class="fas fa-search" aria-hidden="true"></i>
      </span>
    </p>
  </div>
  
  <a class="panel-block" v-for="item,key in lists">
    <span class=" column is-9">
    	 {{ key+1 }}   {{ item.name }}
    </span>
    
    <span class="panel-icon column is-1">
      <i class="fa fa-trash has-text-danger" aria-hidden="true" @click="del(key,item.id)"></i>
    </span>
    <span class="panel-icon column is-1">
      <i class="fa fa-edit has-text-success" aria-hidden="true" @click="openUpdate(key)"></i>
    </span>
    <span class="panel-icon column is-1">
      <i class="fa fa-eye has-text-warning" aria-hidden="true" @click="openShow(key)"></i>
    </span>
  </a>
  
</nav>
<Add :openmodal='addActive' @closeRequest='close'></Add>
<Show :openmodal='showActive' @closeRequest='close'></Show>
<Update :openmodal='updateActive' @closeRequest='close'></Update>
</div>
</template>
<script>
let Add = require('./Add.vue').default;
let Show = require('./Show.vue').default;
let Update = require('./Update.vue').default;
  export default{
  components:{Add,Show,Update},
  data(){
  return{
  addActive:'',
  showActive:'',
  updateActive:'',
  lists:{},
  errors:{},
}
},
mounted(){
axios.post('/getData')
      .then((response)=> this.lists = this.temp = response.data)
      .catch((error) => this.errors = error.response.data.errors)
    
},
  methods:{
  openAdd(){
  this.addActive='is-active';
},
openShow(key){
  this.$children[1].list =this.lists[key]
  this.showActive='is-active';
},
openUpdate(key){
  this.$children[2].list =this.lists[key]
  this.updateActive='is-active';
},
close(){
  this.addActive=this.showActive= this.updateActive='';
},
del(key,id){
  // console.log(`${key} ${id}`)
  if (confirm("are you sure")){
  axios.delete(`/phonebook/${id}`)
      .then((response)=> console.log('deleted'))
      .catch((error) => this.errors = error.response.data.errors)
    }
}
}
}
</script>