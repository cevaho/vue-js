<template>
	<div class="container-form-nomville">

		<!-- @submit.prevent est un event modifier qui empeche le submit de recharger la page
		vmodel est ajouter à l'input, 2-way-binding, 
			permet de charger la nouvelle valeur de l'input sans recharger la page	-->

		<form class="review-form" @submit.prevent="onSubmit">
			<label for="name">Afficher la météo d'une ville :</label>

		      <div class="input-group">
						
				<input id="name" type="text" v-model="name" placeholder="indiquez une ville" class="lead search form-control">
				<div class="input-group-append">
					<button type="submit" value="Submit" class="btn lead btn-info">Chercher</button>
				</div>
			</div>
		</form>
		<span id="raison">(pour des raisons techniques, veuillez utiliser l'anglais dans le champs de recherche)</span>
	</div>
</template>

<script>

export default{
	data(){
		return {
			id:1,
          		name: null,
        		}
		},
	methods:{

		onSubmit() {
      			let formNomVille = {
					id:this.id,
        				name: this.name,
      					}

			//emission d'un event input-submitted dans l'objet forminput,
			//emit sert à envoyer des information d'un enfant vers son parent 
			//car <formNomVille> sera intégré dans un autre component 
			//un appel sera fait  dans une balise contenant cet evenement

      			this.$emit('input-submitted', formNomVille);

			//ensuite on remet name à zero pour le remodifier si l'utilisateur le désire
      			this.name = null;
			this.id++;
    			}

		}
}


</script>
