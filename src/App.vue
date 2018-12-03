<template>
	<div class="container">
		<div id="app">
			<input 
				v-model="search" 
				placeholder="Search by name"
				class="search-bar name-search"
			/>
			
			<input 
				class="search-bar tag-search"
				v-model="tagsearch"
				placeholder="Search by tags"
			/>

			<Student
	            v-for="(profile,index) in filteredList"
	            :id="profile.id"
	            :student="profile"
	            key="index"
	            v-on:add-tag="addTag"
	        />
		</div>
	</div>
</template>

<script>

import Student from './Student.vue'

export default {
	name: 'app',
	props: ['id'],
	data () {
		return {
		    search: '',
		    tagsearch: '',
			data: []
		}
	},
	components:{
	    Student
	},
	created () {
		fetch(`https://www.hatchways.io/api/assessment/students`)
			.then(res => res.json())
			.then(data => {
				this.data = data.students;
				for (let student of this.data) {
					student.fullName = `${student.firstName} ${student.lastName}`;
					student.tags = [	];
				}	
			});
	},
	methods: {
		addTag(params) {
			console.log(params);
			let student = this.data.find((profile) => profile.id === params.id);
			console.log(student);
			student.tags.push(params.tag);
		},
	},
	computed: {
	    filteredList() {
	      return this.data.filter(profile => {
	        return profile.fullName.toLowerCase().includes(this.search.toLowerCase()) && 
	        	((this.tagsearch) ? 
	        		profile.tags.some((tag) => tag.toLowerCase().includes(this.tagsearch.toLowerCase()))
	        		: true)
	      	});
	    }
 	}
}
					


</script>