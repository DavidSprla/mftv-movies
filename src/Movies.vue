<template>
  <div class="movies" :class="{'movies--inspect': inspect}">
		<div class="movies__container">
			<h1 class="movies__title">
				Movie library
			</h1>
			<div class="movies__grid">
				<div v-for="movie in movies" :key="movie.id" class="movies__item">
					<div class="movies__background"></div>
					<button type="button" title="Favourite ♥" class="movies__favourite" :class="{'movies__favourite--active': movie.favourite}" @click="favouriteMovie(movie)">
						<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Capa_1" x="0px" y="0px" viewBox="0 0 512 512" style="enable-background:new 0 0 512 512;" xml:space="preserve" class="movies__favourite-icon">
							<path d="M376,30c-27.783,0-53.255,8.804-75.707,26.168c-21.525,16.647-35.856,37.85-44.293,53.268    c-8.437-15.419-22.768-36.621-44.293-53.268C189.255,38.804,163.783,30,136,30C58.468,30,0,93.417,0,177.514    c0,90.854,72.943,153.015,183.369,247.118c18.752,15.981,40.007,34.095,62.099,53.414C248.38,480.596,252.12,482,256,482    s7.62-1.404,10.532-3.953c22.094-19.322,43.348-37.435,62.111-53.425C439.057,330.529,512,268.368,512,177.514    C512,93.417,453.532,30,376,30z"/>
						</svg>
					</button>
					<div class="movies__info">
						<div class="movies__name">
							{{ movie.title }}
						</div>
					</div>
				</div>
			</div>
		</div>
  </div>
</template>

<script>

export default {
  name: 'Movies',
  data: () => ({
		inspect: false,
    movies: [
			{
				id: 1,
				title: 'The Lord of the Rings: The Fellowship of the Ring',
				favourite: false
			},
			{
				id: 2,
				title: 'The Lord of the Rings: The Two Towers',
				favourite: false
			},
			{
				id: 3,
				title: 'The Lord of the Rings: The Return of the King',
				favourite: false
			},
			{
				id: 4,
				title: 'Saving Private Ryan',
				favourite: false
			},
			{
				id: 5,
				title: 'Alien',
				favourite: false
			},
			{
				id: 6,
				title: 'Back to the Future',
				favourite: false
			},
			{
				id: 7,
				title: 'The Pianist',
				favourite: false
			},
			{
				id: 8,
				title: 'Django Unchained',
				favourite: false
			},
			{
				id: 9,
				title: 'The Dark Knight Rises',
				favourite: false
			},
			{
				id: 10,
				title: 'Taxi Driver',
				favourite: false
			},
			{
				id: 11,
				title: 'Gladiator',
				favourite: false
			},
			{
				id: 12,
				title: 'The Green Mile',
				favourite: false
			},
			{
				id: 13,
				title: 'WALL·E',
				favourite: false
			},
			{
				id: 14,
				title: 'The Lion King',
				favourite: false
			},
			{
				id: 15,
				title: 'Ratatouille',
				favourite: false
			},
			{
				id: 16,
				title: 'Crocodile Dundee',
				favourite: false
			},
			{
				id: 17,
				title: 'Scarface',
				favourite: false
			},
			{
				id: 18,
				title: 'The Silence of the Lambs',
				favourite: false
			},
			{
				id: 19,
				title: 'Pulp Fiction',
				favourite: false
			},
			{
				id: 20,
				title: 'Midnight Express',
				favourite: false
			},
			{
				id: 21,
				title: 'Midnight in Paris',
				favourite: false
			},
			{
				id: 22,
				title: 'L.A. Confidential',
				favourite: false
			},
			{
				id: 23,
				title: 'The Hangover',
				favourite: false
			},
			{
				id: 24,
				title: 'Gran Torino',
				favourite: false
			},
			{
				id: 25,
				title: 'Flight',
				favourite: false
			},
			{
				id: 26,
				title: 'Requiem for a Dream',
				favourite: false
			},
			{
				id: 27,
				title: 'Forrest Gump',
				favourite: false
			},
			{
				id: 28,
				title: 'Madagascar',
				favourite: false
			},
			{
				id: 29,
				title: 'Despicable Me',
				favourite: false
			},
			{
				id: 30,
				title: 'Catch Me If You Can',
				favourite: false
			}
		]
  }),
	methods: {
		favouriteMovie(movie) {

			const moviesStorage = JSON.parse(localStorage.getItem('movies')) || []
			let movieIndex = this.movies.findIndex(movieItem => movieItem.id === movie.id)

			if (!moviesStorage.find(movieStorage => movieStorage.id === movie.id)) {
				moviesStorage.push(movie)
				this.movies[movieIndex].favourite = true
			} else {
				moviesStorage.splice(moviesStorage.indexOf(moviesStorage.find(movieStorage => movieStorage.id === movie.id)), 1)
				this.movies[movieIndex].favourite = false
			}

			moviesStorage.sort((a, b) => {
				return a.id - b.id
			})
			
			localStorage.setItem('movies', JSON.stringify(moviesStorage))

			const favouriteCustomEvent = new CustomEvent('favouriteEvent', {
				detail: moviesStorage.length
			})
			window.dispatchEvent(favouriteCustomEvent)
		},
		handleInspect(event) {
			this.inspect = event.detail 
		}
	},
	mounted() {
		window.addEventListener('inspectEvent', this.handleInspect)

		const moviesStorage = JSON.parse(localStorage.getItem('movies')) || []
		moviesStorage.forEach(movieStorage => {
			this.movies[this.movies.findIndex(movieItem => movieItem.id === movieStorage.id)].favourite = 1
		})

		const inspectStorage = JSON.parse(localStorage.getItem('inspect')) || false
		this.inspect = inspectStorage
	},
	destroyed() {
		window.removeEventListener('inspectEvent', this.handleInspect)
	}
}
</script>

<style scoped>
	.movies{
		position: relative;
		margin: 3rem 1.5rem;
		transition: var(--transition);
	}
	.movies--inspect{
		padding-top: 3rem;
		border-radius: 1rem;
		box-shadow: 0 0 .6rem .1rem var(--vue);
	}
	.movies--inspect::before{
		position: absolute;
		content: 'movies (Vue)';
		top: 1rem;
		left: 1.5rem;
		color: var(--vue);
	}
	.movies__container{
		max-width: 160rem;
		padding: 1.5rem;
		margin: 0 auto;
	}
	.movies__title{
		margin: 0;
		padding-bottom: 1rem;
		font-size: 4rem;
		font-weight: 700;
		color: var(--dark-gray);
		letter-spacing: .02em;
		text-transform: uppercase;
		text-align: center;
		user-select: none;
		cursor: default;
	}
	@media only screen and (min-width: 768px) {
		.movies__title{
			text-align: left;
		}
	}
	.movies__grid{
		display: grid;
		grid-template-columns: repeat(1, 1fr);
		row-gap: 1rem;
	}
	@media only screen and (min-width: 425px) {
		.movies__grid{
			grid-template-columns: repeat(2, 1fr);
			column-gap: 1rem;
		}
	}
	@media only screen and (min-width: 768px) {
		.movies__grid{
			grid-template-columns: repeat(3, 1fr);
			column-gap: 3rem;
			row-gap: 3rem;
		}
	}
	@media only screen and (min-width: 1024px) {
		.movies__grid{
			grid-template-columns: repeat(4, 1fr);
		}
	}
	@media only screen and (min-width: 1440px) {
		.movies__grid{
			grid-template-columns: repeat(5, 1fr);
		}
	}
	.movies__item{
		position: relative;
		min-height: 20rem;
		padding: 1rem;
		border-radius: 1rem;
		overflow: hidden;
		transition: var(--transition);
	}
	.movies__item:hover{
		box-shadow: 0 0 1rem var(--gray);
	}
	.movies__background{
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background-image: url("https://picsum.photos/id/184/400/300");
		background-repeat: no-repeat;
		background-position: bottom;
		border-radius: 1rem;
	}
	.movies__favourite{
		position: absolute;
		top: 1rem;
		right: 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: .6rem;
		background: rgba(0,0,0,0.5);
		border-radius: 50%;
		box-shadow: none;
		border: none;
		cursor: pointer;
		outline: none;
	}
	.movies__favourite--active .movies__favourite-icon{
		fill: var(--red);
	}
	.movies__favourite-icon{
		fill: var(--light-gray);
		width: 1.8rem;
		height: 1.8rem;
	}
	.movies__info{
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		padding: 1.5rem 1rem;
		background: rgba(0,0,0,0.5);
		user-select: none;
	}
	.movies__name{
		font-weight: 500;
		color: var(--white);
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
</style>