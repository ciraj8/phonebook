<template>
	<div class="modal" :class='openmodal'>
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Add Entries</p>
      <button class="delete" aria-label="close" @click="close()"></button>
    </header>
    <section class="modal-card-body">
      <div class="field">
		  <label>Name</label>
		  <div class="control">
		    <input class="input is-primary" :class=" {'is-danger':errors.name }" type="text" placeholder="Enter Name" v-model='list.name'>
		  </div>
		  <small v-if="errors.name" class="has-text-danger">{{ errors.name[0]}}</small>
		</div>
		<div class="field">
		  <label>Phone</label>
		  <div class="control">
		    <input class="input is-primary" :class=" {'is-danger':errors.name }" type="number" placeholder="Enter Phone number" v-model='list.phone'>
		  </div>
		  <small v-if="errors.name" class="has-text-danger">{{ errors.phone[0]}}</small>
		</div>
		<div class="field">
		  <label>Email</label>
		  <div class="control">
		    <input class="input is-primary" :class=" {'is-danger':errors.name }" type="email" placeholder="Enter Email" v-model='list.email'>
		  </div>
		  <small v-if="errors.name" class="has-text-danger">{{ errors.email[0]}}</small>
		</div>
    </section>
    <footer class="modal-card-foot">
      <button class="button is-success" @click="save">Save</button>
      <button class="button" @click="close">Cancel</button>
    </footer>
  </div>
</div>
</template>

<script>
	 export default{
	 	props:['openmodal'],
	 	data(){
	 		return{
	 			list:{
	 				name:'',
	 				email:'',
	 				phone:'',
	 			},
	 			errors:{},
	 		
	 		}

	 	},

	 	methods:{
	 		close(){
	 		this.$emit('closeRequest')
	 	},
	 	save(){
           axios.post('/phonebook',this.$data.list).then((response)=>
			    console.log(response))
			  .catch((error) =>
			    this.errors =error.response.data.errors)
		
	 	},
	 	}
	 };
</script>