
<template>
	<div class="student">
		<img class="student-img" :src="student.pic" :alt="student.fullName">
		<div class="profile">
			<h1 class="name"><strong>{{ student.fullName }}</strong></h1>
			<p>Email: {{ student.email }}</p>
			<p>Company: {{ student.company }}</p>
			<p>Skill: {{ student.skill }}</p>
			<p>Grade average: {{avg}}%</p><br><br>

			<div class="test-score hidden">
		        <table>
		        	<tr v-for="(grade, index) in student.grades">
		        		<td>Test {{index+1}}</td>
		        		<td>{{ grade }}</td>
		        	</tr>
		        </table><br>

		        <div class="tag-list">
					<span
						v-for="(tag, index) in student.tags"
						class="tag"
					>
						{{tag}}
					</span>
				</div>
				<input
					class="add-tag"
					placeholder="Add a tag"
					v-model="newTag"
					@keyup.enter="submit"
				/>
			</div>

			
		</div>
		<button class="collapse" @click="expand">+</button>
	</div>
</template>

<script>

export default {
	props: ['id', 'student'],
	data () {
		return {
			newTag: ''
		}
	},
	computed: {
		avg() {
    		let grades = this.student.grades;
			return grades.reduce((a,b) => parseInt(a) + parseInt(b)) / grades.length;
		}
	},
	methods: {
		expand (event) {
			let target = event.target.parentElement.querySelector("div.test-score");
		   	target.classList.toggle("hidden");

		   	let content = event.target.textContent;
			event.target.textContent = (content === '+') ? "–" : "+";
		},
		submit (event) {
			this.$emit('add-tag',{id: this.id, tag:this.newTag});
			this.newTag = '';
		}
	}
}
</script>