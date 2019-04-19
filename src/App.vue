<template>
  <div id="app" class="container-fluid">
	<div class="row">
		<div class="col-12 col-md-6">
		    <div v-if="arrayToday.length" id="dayone">
			<h2><img v-bind:src='onlyFive[0][0]["icon"]' > {{ onlyFive[0][0]["ville"] }}</h2>
			<h3>Ce {{ onlyFive[0][0]["jour"] }}</h3>
			<p>{{ onlyFive[0][0]["date"] }} à {{ onlyFive[0][0]["heure"] }}h</p>
			<p>T° minimum : <span id="minz">{{ onlyFive[0][0]["temp_min"]}}</span> °c, maximum : <span id="maxz">{{ onlyFive[0][0]["temp_max"]}}</span> °c</p>
			<p>Météo du jour : {{ onlyFive[0][0]["meteo"] }},<br> vent : {{onlyFive[0][0]["vent"]}} BFT</p>
	       	    </div>

		    <header>
				<h1>Nos prévisions météo</h1>
				<p>utilisant openWeathermap API avec Vue.js</p>
				<form_no_ville @input-submitted="citysearch"></form_no_ville>
		    </header>

		</div>



	<section v-if="arrayToday.length" class="col-12 col-md-6">
	    <div id="sectionner">
		<h3>Prévisions pour les 4 prochains jours : </h3>
		<!-- affiche les jours des lignes de onlyFive[1] donc du deuxième jour-->
		<article id="daytwo">
			<table>
			    <tbody>
				<tr>
					<th>&nbsp;{{ onlyFive[1][0].jour }}</th>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[1]">{{itemas.heure}} h</td>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[1]"><img v-bind:src='itemas.icon' ></td>
				</tr>
				<tr class="temper">
					<td v-for="itemas in onlyFive[1]">max<br><span>{{itemas.temp_max}}</span><br>°C</td>
				</tr>
			    </tbody>
			</table>
		</article>

		<!-- affiche les jours des lignes de onlyFive[2] donc du troisième jour-->
		<article v-if="arrayToday.length" id="daythree">
			<table>
			    <tbody>
				<tr>
					<th>&nbsp;{{ onlyFive[2][0].jour }}</th>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[2]">{{itemas.heure}} h</td>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[2]"><img v-bind:src='itemas.icon' ></td>
				</tr>
				<tr class="temper">
					<td v-for="itemas in onlyFive[2]">max<br><span>{{itemas.temp_max}}</span><br>°C</td>
				</tr>
			    </tbody>
			</table>
		</article>

		<!-- affiche les jours des lignes de onlyFive[3] donc du quatrième jour-->
		<article v-if="arrayToday.length" id="dayfour">
			<table>
			    <tbody>
				<tr>
					<th>&nbsp;{{ onlyFive[3][0].jour }}</th>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[3]">{{itemas.heure}} h</td>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[3]"><img v-bind:src='itemas.icon' ></td>
				</tr>
				<tr class="temper">
					<td v-for="itemas in onlyFive[3]">max<br><span>{{itemas.temp_max}}</span><br>°C</td>
				</tr>
			    </tbody>
			</table>
		</article>

		<!-- affiche les jours des lignes de onlyFive[4] donc du cinquième jour-->
		<article v-if="arrayToday.length" id="dayfive">
			<table>
			    <tbody>
				<tr>
					<th>&nbsp;{{ onlyFive[4][0].jour }}</th>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[4]">{{itemas.heure}} h</td>
				</tr>
				<tr>
					<td v-for="itemas in onlyFive[4]"><img v-bind:src='itemas.icon' ></td>
				</tr>
				<tr class="temper">
					<td v-for="itemas in onlyFive[4]">max<br><span>{{itemas.temp_max}}</span><br>°C</td>
				</tr>
			    </tbody>
			</table>
		</article>
	    </div>
	</section>

     </div>
	<!--img src="http://www.fondsecran.eu/a/get_photo/155614/1280/800" width="100%" id="bg"-->
  </div>
</template>

<script>

import form_no_ville from './components/form-nomville.vue'
//import axios from 'axios'
//import TimelineMax from 'gsap/TimelineMax'
//import TimelineLite from 'gsap'

export default {

  name: 'app',

  components:{
		form_no_ville
	},

  data () {
    return {
      msg: 'weatherforecast app test',
      villeNom:"",
      arrayToday:[],
      weather:{},
      dayUn:[],
      dayDeux:[],
      dayTrois:[],
      dayQuatre:[],
      dayCinq:[],
      daySix:[],
      dayZero:[],
      onlyFive:[]
    }
  },

  methods:{
	citysearch(villeNom){
			console.log("click recherche "+villeNom.name+" et commence le script");

			let apiUrlForecast = "http://api.openweathermap.org/data/2.5/forecast?q="
						+villeNom.name+"&units=metric&lang=fr&APPID=61287c53f07c71bc6054a216ecca6f89"

			//réinitialiser la valeur des tableaux si un recherche existait déjà
			this.arrayToday=[];this.onlyFive=[];
			this.dayUn=[];this.dayDeux=[];this.dayTrois=[];this.dayQuatre=[];
			this.dayCinq=[];this.daySix=[];this.dayZero=[];

			this.axios.get(apiUrlForecast).then((response) => {

		  	console.log("juste après l'appel affiche le nombre de ligne du tableau : "+(response.data.list).length)
			console.log("jour zero actuel : "+response.data.list[0].dt_txt)

			/*
			traitement des données relative à la date et au jour
			*/
				function monjour(val){
								let heure2 = val.slice(0,10)
								let dayer = new Date(heure2);
									console.log("jour de ligne : "+dayer.getDay())
								var weekday = new Array(7);
									weekday[0] = "Dimanche";
									weekday[1] = "Lundi";
									weekday[2] = "Mardi";
									weekday[3] = "Mercredi";
									weekday[4] = "Jeudi";
									weekday[5] = "Vendredi";
									weekday[6] = "Samedi";

								var jour = weekday[dayer.getDay()];
								return jour}
			/*
			placement de l'objet weather dans l'arrayToday
			qui contient les 40 résultats,8 résultats par jour, un toutes les 3h
			*/
				for(let i=0;i< response.data.list.length;i++){

					this.arrayToday.push(

						this.weather = {
							ville:response.data.city.name,
							temp:response.data.list[i].main.temp,
							temp_max:response.data.list[i].main.temp_max,
							temp_min:response.data.list[i].main.temp_min,
							meteo:response.data.list[i].weather[0].description,
							vent:response.data.list[i].wind.speed,
							icon:"http://openweathermap.org/img/w/"+response.data.list[i].weather[0].icon+".png",
							date:(response.data.list[i].dt_txt).slice(0,10),
							heure:(response.data.list[i].dt_txt).slice(11, 13),
							jour: monjour(response.data.list[i].dt_txt)
						}

					)
				}
				console.log("affiche le dernier objet weather contenu dans arrayToday")
				console.log(this.weather)
				console.log("le dernier jour affiché sera : ")
				console.log(this.weather.jour)

				console.log("affiche l'array non filtré qui contient weather")
				console.log(this.arrayToday)

			/*
			split du big array en petits array par jour
			*/
				for(let i=0;i< response.data.list.length;i++){
					let myDay=this.arrayToday[i].jour

					switch(myDay){
						case "Lundi":this.dayUn.push(this.arrayToday[i]);break;
						case "Mardi":this.dayDeux.push(this.arrayToday[i]);break;
						case "Mercredi":this.dayTrois.push(this.arrayToday[i]);break;
						case "Jeudi":this.dayQuatre.push(this.arrayToday[i]);break;
						case "Vendredi":this.dayCinq.push(this.arrayToday[i]);break;
						case "Samedi":this.daySix.push(this.arrayToday[i]);break;
						case "Dimanche":this.dayZero.push(this.arrayToday[i]);
						}

				}


				//console.log(this.dayQuatre)
				//switch day after


				//let jeudiDay = this.arrayToday.find(this.weather.jour)
				//console.log(jeudiDay)

					let myDay=this.arrayToday[0].jour

					switch(myDay){
						case "Lundi":this.onlyFive.push
(this.dayUn,this.dayDeux,this.dayTrois,this.dayQuatre,this.dayCinq,this.daySix);
						break;
						case "Mardi":this.onlyFive.push
(this.dayDeux,this.dayTrois,this.dayQuatre,this.dayCinq,this.daySix,this.dayZero);
						break;
						case "Mercredi":this.onlyFive.push
(this.dayTrois,this.dayQuatre,this.dayCinq,this.daySix,this.dayZero,this.dayUn);
						break;
						case "Jeudi":this.onlyFive.push
(this.dayQuatre,this.dayCinq,this.daySix,this.dayZero,this.dayUn,this.dayDeux);
						break;
						case "Vendredi":this.onlyFive.push
(this.dayCinq,this.daySix,this.dayZero,this.dayUn,this.dayDeux,this.dayTrois);
						break;
						case "Samedi":this.onlyFive.push
(this.daySix,this.dayZero,this.dayUn,this.dayDeux,this.dayTrois,this.dayQuatre);
						break;
						case "dimanche":this.onlyFive.push
(this.dayZero,this.dayUn,this.dayDeux,this.dayTrois,this.dayQuatre,this.dayCinq);
						}

				console.log("array global:")
				console.log(this.onlyFive)


				})

		}

  },

  computed:{



  },

  updated(){


	/*
	Change les couleur des températures les plus élevée et plus faible parmis chaque tableau
	*/
	let higher =[]
	let topval;
	let minval;

	let countArticle = document.querySelectorAll('section article').length;
	console.log(countArticle);

	let temperColor2 = document.querySelectorAll('#daytwo .temper span');
	let temperColor3 = document.querySelectorAll('#daythree .temper span');
	let temperColor4 = document.querySelectorAll('#dayfour .temper span');
	let temperColor5 = document.querySelectorAll('#dayfive .temper span');

	temperColor2.forEach(function(el) {addInArray(el);});
	temperColor2.forEach(function(el) {colorChanger(el);});
	higher =[];
	temperColor3.forEach(function(el) {addInArray(el);});
	temperColor3.forEach(function(el) {colorChanger(el);});
	higher =[];
	temperColor4.forEach(function(el) {addInArray(el);});
	temperColor4.forEach(function(el) {colorChanger(el);});
	higher =[];
	temperColor5.forEach(function(el) {addInArray(el);});
	temperColor5.forEach(function(el) {colorChanger(el);});



	function addInArray(el){
				higher.push(Number(el.innerHTML));
				topval=Math.max.apply(null, higher);
				minval=Math.min.apply(null, higher);
				};

	function colorChanger(el){
				if(Number(el.innerHTML) == topval){
							el.classList.add("maxeur");
  							}
				if(Number(el.innerHTML) == minval){
							el.classList.add("mineur");
							}
				};

	//animations gsap qui bug avevc un loop infinite
	//const timeline = new TimelineLite();
	//let sectionner = document.querySelector('#sectionner');
	//let sectionner = document.querySelector('section');

		/*timeline.from(sectionner, 0.6, {width:"0px",overflow:"hidden"})
		timeline.to(sectionner,2.5, {width:"100%",overflow:"auto"})*/


  }
}

</script>

<style>
html{background: url('http://www.fondsecran.eu/a/get_photo/155614/1280/800') no-repeat center top fixed black;}
body{color:#003380;background-color:transparent;}
header,div#dayone,#sectionner{padding: 15px;background-color: white;margin-top: 15px;margin-bottom:15px;border-radius: 0.25rem;}
article {border-bottom:1px solid #0099e6;margin-bottom:20px;padding-bottom: 20px;}

header h1{font-size:2rem;}
html body h3{font-size:1.5rem;}
body h1,body h2,body h3,body h4{font-family:"Palatino Linotype", "Book Antiqua", Palatino, serif;}

body .btn-info{background-color:#003380;}
body .btn-info:hover{background-color:#0047b3;}

#raison{font-size:9px;letter-spacing:0.05rem;color:#75a3a3;}


article table{width: 100%;}
article th{font-weight: normal;font-style: italic;}
article td{font-size:10px;width:12.5%;text-align: center;}
article td img{width:100%;max-width:52px;}
.temper td{color:#a6a6a6;}
.temper td span{font-size:14px;color:#5c8a8a;}

span#minz{color:#80d4ff;font-weight:bold;}
span#maxz{color:red;font-weight:bold;}
.temper span.mineur{font-weight:bold;color:#80d4ff;padding:3px 4px 2px;border:1px solid #0099e6;}
.temper span.maxeur{font-weight:bold;color:red;padding:3px 4px 2px;border:1px solid red;}

#sectionner{
    -webkit-animation: 2s ease 0s normal forwards 1 fadein;
    animation: 2s ease 0s normal forwards 1 fadein;
}

#dayone{
    -webkit-animation: 1s ease 0s normal forwards 1 fadein;
    animation: 1s ease 0s normal forwards 1 fadein;
}

@keyframes fadein{
    0% { opacity:0; }
    100% { opacity:1; }
}

@-webkit-keyframes fadein{
    0% { opacity:0; }
    100% { opacity:1; }
}


#bg{position:fixed;}
@media (max-width:576px){/*.container{❝max❞-width:540px}*/
td{font-size:8px;}
.temper td{color:#a6a6a6;}
.temper td span{font-size:10px;color:#5c8a8a;}
}
@media (min-width:768px){/*.container{❝max❞-width:720px}*/

}
@media (min-width:992px){/*.container{❝max❞-width:960px}*/

}
@media (min-width:1200px){/*.container{❝max❞-width:1140px}*/

}
</style>
