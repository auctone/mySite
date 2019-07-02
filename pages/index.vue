<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <v-card>
        <v-card-title class="headline">
          Mikes
        </v-card-title >
			  <v-img
				  v-for="index in totalCards"
				  :key="index" class="cards"
				  height="150px"
				  width="110px"
				  :data-index="index"
				  :id="'random-card' + index"
				  :src="deal.data.cards[index -1].image">
			  </v-img>
		  		<v-text-field
					type="number"
					label="How many cards to show"
					mask="#"
					@input="up"
				>

				</v-text-field>
		  {{deal}}
        <v-card-actions>
          <v-spacer />
          <v-btn
           @click="newCard()"
          >New Card
          </v-btn>
			<v-btn
				@click="shuffleAgain()"
			>Shuffle
			</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
	async asyncData ({ params }) {


		let shuffle = await axios.get('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1');
		let deal = await axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/draw/?count=7');
		let reShuffle = await axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/shuffle/');
		let newDeck = await axios.get('https://deckofcardsapi.com/api/deck/new/');
		//let piles = await axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/pile/mike/add/?cards=AS,2S');
		//let listPiles = await axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/pile/mike/list');
		//let drawFromPiles = await axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/pile/main/list');
		return { shuffle, deal, reShuffle, newDeck}
	},
  components: {
    Logo
  },
	data() {
		return {
			loading: false,
			rows: [],
			totalCards: 7
		}
	},
	methods: {
		newCard() {
			axios.get(`https://deckofcardsapi.com/api/deck/oc4eusqclle8/draw/?count=${this.totalCards}`).then(data=>{
				this.deal = data;
			})

		},
		shuffleAgain(){
			axios.get('https://deckofcardsapi.com/api/deck/oc4eusqclle8/shuffle/').then(data=>{
				this.reShuffle = data;
			})
		},
		up (){

		},
  }
}
</script>
<style >
.cards {

	position: relative;
}

</style>