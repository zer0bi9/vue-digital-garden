---
{"dg-publish":true,"permalink":"/vuejs-example/","tags":"gardenEntry"}
---


# Movie App
## Vue Router

> vue-router install : npm i vue-router@4

src/routes/index.js
```js
import {createRouter, createWebHashHistory} from 'vue-router'
import Home from './Home'
import Movie from './Movie'
import About from './About'
  
export default createRouter({
// Hash mode
// https://google.com/#/search <- url 쪽에 표시되는 방법
history: createWebHashHistory(),

// pages
routes: [
{
path: '/',
component: Home
},

{
path: '/movie',
component: Movie
},

{
path: '/about',
component: About
}]
})
```

* routes 하위에 Home, Movie, About 컴포넌트 생성(단순!)


src/app.vue
```vue
<template>
<RouterView />
</template>
....
```

## Bootstrap
> bootstrap install : npm i bootstrap

src/scss/main.scss
```scss
// Default variable overrides
$primary: #FDC000; // 메인(primary) 컬러 변경 blue -> #FDC000

// Required
@import "../../node_modules/bootstrap/scss/functions";
@import "../../node_modules/bootstrap/scss/variables";
@import "../../node_modules/bootstrap/scss/mixins";
@import "../../node_modules/bootstrap/scss/bootstrap"
```

src/scss/main.scss
```scss
....
<style lang="scss">
@import "~/scss/main.scss"
</style>
```

- reference
	[Bootstrap](https://getbootstrap.com/docs/5.2/getting-started/introduction/)



## Header - nav
- 공통으로 사용될 Header 구성

src/components/Header.vue
```vue
<template>
<header>

<div
class="nav nav-pills">

<div 
v-for="nav in navigations"
:key="nav.name"
active-class="active"
class="nav-item">

<RouterLink
:to="nav.href"
class="nav-link">
{{ nav.name }}
</RouterLink>
</div>
</div>
</header>
</template>

<script>
export default {
data() {
return {
navigations: [
{
name: 'Search',
href: '/'
},
{
name: 'Movie',
href: '/movie'
},
{
name: 'About',
href: '/about'
}]
}
}
}
</script>
<style lang="scss" scoped>
</style>
```

- reference
[router-vue](https://router.vuejs.org/installation.html)
[nav&tabs](https://getbootstrap.com/docs/5.2/components/navs-tabs/)

* Header 연결
src/App.vue
```vue
<template>
<Header /> <!-- Header 컴포넌트 추가 -->
<RouterView />
</template>
....
```


## Header - Logo & Google Fonts

- Logo를 위한 Fonts 구성
	1. https://fonts.google.com/ 에 접속
	2. 원하는 폰트를 선택하여 link 주소 획득
	3. index.html의 head에 연결
/index.html
```html
....
<head>
....
<link rel="preconnect" href="https://fonts.googleapis.com">  
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>  
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@500&family=Roboto:wght@400;700&display=swap" rel="stylesheet">

<style>
	body {
	line-height: 1.4;
	font-family: 'Roboto', sans-serif;
	}
</style>
....
```

src/components/Logo.vue
- RouterLink를 활용하여 전환위치 설정
- Logo 컴포넌트 style 적용
```vue
<template>
  <RouterLink
    to="/"
    class="logo">
    <span>OMDbAPI</span>.COM
  </RouterLink>
</template>

<script>
export default {
}
</script>

<style lang="scss" scoped>
@import "~/scss/main";

.logo {
font-family: "Oswald",sans-serif;
font-size: 20px;
color: $black;
text-decoration: none;
 &:hover {
 color: $black;
 }
 span {
  color: $primary;
 }
}

</style>
```

src/components/Header.vue
- Header 컴포넌트에 Logo 컴포넌트를 연결
- Header 스타일 및 Header 안의 logo class 스타일 적용
```vue
....
<header>
  <Logo />
....
</header>

<script>
import Logo from '~/components/Logo'
export default {
 components: {
  Logo
},
....
}
</script>

<style lang="scss" scoped>
 header {
  height: 70px;
  padding: 0 40px;
  display: flex;
  align-items: center;
 .logo {
  margin-right: 40px;
 }
}
</style>
```
- reference
[Google Fonts](https://fonts.google.com/)

## HeadLine
src/components/Headline.vue
- headline에 쓰여질 문구를 OMDbapi.com으로부터 복사
- h1 & p 태그의 스타일 적용
	- line-height: 1 설정 <- 줄 높이를 1배로 설정하는데 h1의 내용은 글자가 커지고 높이가 높아지면 간격이 넓어지기 때문
- bootstrap Contianers를 이용 - 중앙정렬(기존의 innner를 이용하던 방법), 가변으로 정렬 가능, break point를 기준으로 변경, 반응형 레이아웃
- container class의 위쪽 내부 여백 추가
```vue
<template>
 <div class="container">
  <h1>
   <span>OMDb API</span><br />
   THE OPEN<br />
   MOVIE DATABASE
  </h1>
  <p>
  The OMDb API is a RESTful web service to obtain movie information, all content and images on the site are contributed and maintained by our users.<br />
  If you find this service useful, please consider making a one-time donation or become a patron.
  </p>
</div>
</template>

<style lang="scss" scoped>
@import "~/scss/main";
.container {
 padding-top: 40px;
}
h1 {
 line-height: 1;
 font-family: "Oswald", sans-serif;
 font-size: 80px;
span {
 color: $primary;
}

p {
 margin: 30px 0;
 color: $gray-600;
}}
</style>
```

src/routes/Home.vue
- Headline 컴포넌트 연결
```vue
<template>
 <Headline />
</template>
....
```

- reference
[Bootstrap - Containers](https://getbootstrap.com/docs/5.2/layout/containers/)


## Search - Filter
src/components/Search.vue
- Bootstrap의 form control를 이용하여 input style 지정
- Bootstrap의 form 카테고리의 select를 이용하여 동일한 요소를 적용
- v-model에 $data를 이용하면 export 한 데이터 안의 type에 접근이 가능
- select의 style 지정
- v-for를 이용한 select의 option에 All years 를 적용 (item year에만 출력하도록 v-if 활용)
```vue
<template>
	<div class="container">
		<input
			class="form-control"
			v-model="title"
			type="text"
			placeholder="Search for Movies, Series & more" />
		<div class="selects">
			<select
				v-for="filter in filters"
				v-model="$data[filter.name]"
				:key="filter.name"
				class="form-select">
					<option
						v-if="filter.name === 'year'"
						value="">
						All Years
					</option>
					<option
						v-for="item in filter.items"
						:key="item">
						{{ item }}
					</option>
			</select>
		</div>
	</div>
</template>

<script>
export default {
	data() {
	  return {
			title: '',
			type: 'movie',
			number: 10,
			year: '',
			filters: [
				{
					name: 'type',
					items: ['movie', 'series', 'episode'],
				},
			{
				name: 'number',
				items: [10, 20, 30]
			},
			{
				name: 'year',
				items: (() => {
				const years = []
				const thisYear = new Date().getFullYear()
				for (let i = thisYear; i >= 1985; i -= 1) {
				years.push(i)
				}
				return years
				})()
			}]
		}
	}
}
</script>

<style lang="scss" scoped>
	.container {
		display: flex;
			>	* {
			margin-right: 10px;
			font-size: 15px;
			&:last-child {
				margin-right: 0px;
			}
		}
		.selects {
			display: flex;
			select {
			width: 120px;
			margin-right: 10px;
			&:last-child {
				margin-right: 0px;
			}
		}
	}
}
</style>
```

- reference
[Bootstrap - Forms - Form control](https://getbootstrap.com/docs/5.2/forms/form-control/)
[Bootstrap -Forms - Select](https://getbootstrap.com/docs/5.2/forms/select/)

## Search - Button
src/components/Search.vue
- Apply 버튼 및 apply methods 생성
- apply는 input 요소에도 사용
- apply 버튼이 flex에 의해 너비 감소를 방지하기 위해 flex-shrink를 0으로 셋팅
- axios module을 이용하여 api 활용(설치 필요)
```vue
<template>
	<div class="container">
		<input
			class="form-control"
			v-model="title"
			type="text"
			placeholder="Search for Movies, Series & more"
			@keyup.enter="apply" />
		....
		<button
			class="btn btn-primary"
			@click="apply">
			Apply
		</button>
		....
</template>
<script>
	....
	methods: {
		async apply() {
		// Search movies..
		const OMDB_API_KEY = '7035c60c'
		const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${this.title}&type=${this.type}&y=${this.year}&page=1`)
		console.log(res)
		}
	}
	....
</script>
<style lang="scss" scoped>
	.container {
	....
	.btn {
		width: 120px;
		height: 50px;
		font-weight: 700;
		flex-shrink: 0;
		}
	}
</style>
```

api 호출 이후 axios result 결과
```json
{
    "data": {
        "Search": [
            {
                "Title": "Frozen",
                "Year": "2013",
                "imdbID": "tt2294629",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMTQ1MjQwMTE5OF5BMl5BanBnXkFtZTgwNjk3MTcyMDE@._V1_SX300.jpg"
            },
            {
                "Title": "Frozen II",
                "Year": "2019",
                "imdbID": "tt4520988",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMjA0YjYyZGMtN2U0Ni00YmY4LWJkZTItYTMyMjY3NGYyMTJkXkEyXkFqcGdeQXVyNDg4NjY5OTQ@._V1_SX300.jpg"
            },
            {
                "Title": "Frozen",
                "Year": "2010",
                "imdbID": "tt1323045",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMTc5MTg0ODgxMF5BMl5BanBnXkFtZTcwODEzOTYwMw@@._V1_SX300.jpg"
            },
            {
                "Title": "The Frozen Ground",
                "Year": "2013",
                "imdbID": "tt2005374",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BYzM3Mjc1ZDItMTE1OC00ODk0LWFmZjctYzgxZmYwNzliMTdkXkEyXkFqcGdeQXVyMTAxNDE3MTE5._V1_SX300.jpg"
            },
            {
                "Title": "Frozen River",
                "Year": "2008",
                "imdbID": "tt0978759",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMTk2NjMwMDgzNF5BMl5BanBnXkFtZTcwMDY0NDY3MQ@@._V1_SX300.jpg"
            },
            {
                "Title": "Frozen Fever",
                "Year": "2015",
                "imdbID": "tt4007502",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMjY3YTk5MjUtODBjOC00NzAwLTgyYjYtMzFmMzAxOTZmOWRlXkEyXkFqcGdeQXVyNDgyODgxNjE@._V1_SX300.jpg"
            },
            {
                "Title": "Olaf's Frozen Adventure",
                "Year": "2017",
                "imdbID": "tt5452780",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BOWQ1NjNiZTEtYzc3Zi00Nzk4LTg5MTYtNzc5NmJjYTg1MGQ4XkEyXkFqcGdeQXVyMTA4NDI1NTQx._V1_SX300.jpg"
            },
            {
                "Title": "Frozen Stiff",
                "Year": "2002",
                "imdbID": "tt0301634",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMTk5NDc0MjU3Nl5BMl5BanBnXkFtZTcwNDc3NTU3OQ@@._V1_SX300.jpg"
            },
            {
                "Title": "Frozen Land",
                "Year": "2005",
                "imdbID": "tt0388318",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMTczMjgyMjQxNF5BMl5BanBnXkFtZTcwNjUxMjU3Mg@@._V1_SX300.jpg"
            },
            {
                "Title": "A Frozen Flower",
                "Year": "2008",
                "imdbID": "tt1155053",
                "Type": "movie",
                "Poster": "https://m.media-amazon.com/images/M/MV5BMWZhNjQ4ZTItNDM0NS00NGEzLWE5YjAtMmJmM2RhMTMwNzNkXkEyXkFqcGdeQXVyMzQ2MDUxMTg@._V1_SX300.jpg"
            }
        ],
        "totalResults": "310",
        "Response": "True"
    },
    "status": 200,
    "statusText": "",
    "headers": {
        "cache-control": "public, max-age=86400",
        "content-type": "application/json; charset=utf-8",
        "expires": "Mon, 03 Oct 2022 08:03:33 GMT",
        "last-modified": "Mon, 03 Oct 2022 07:03:33 GMT"
    },
    "config": {
        "transitional": {
            "silentJSONParsing": true,
            "forcedJSONParsing": true,
            "clarifyTimeoutError": false
        },
        "transformRequest": [
            null
        ],
        "transformResponse": [
            null
        ],
        "timeout": 0,
        "xsrfCookieName": "XSRF-TOKEN",
        "xsrfHeaderName": "X-XSRF-TOKEN",
        "maxContentLength": -1,
        "maxBodyLength": -1,
        "env": {
            "FormData": null
        },
        "headers": {
            "Accept": "application/json, text/plain, */*"
        },
        "method": "get",
        "url": "https://www.omdbapi.com/?apikey=7035c60c&s=frozen&type=movie&y=&page=1"
    },
    "request": {}
}
```

web의 response 값
```json
{"Search":[{"Title":"Frozen","Year":"2013","imdbID":"tt2294629","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMTQ1MjQwMTE5OF5BMl5BanBnXkFtZTgwNjk3MTcyMDE@._V1_SX300.jpg"},{"Title":"Frozen II","Year":"2019","imdbID":"tt4520988","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMjA0YjYyZGMtN2U0Ni00YmY4LWJkZTItYTMyMjY3NGYyMTJkXkEyXkFqcGdeQXVyNDg4NjY5OTQ@._V1_SX300.jpg"},{"Title":"Frozen","Year":"2010","imdbID":"tt1323045","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMTc5MTg0ODgxMF5BMl5BanBnXkFtZTcwODEzOTYwMw@@._V1_SX300.jpg"},{"Title":"The Frozen Ground","Year":"2013","imdbID":"tt2005374","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BYzM3Mjc1ZDItMTE1OC00ODk0LWFmZjctYzgxZmYwNzliMTdkXkEyXkFqcGdeQXVyMTAxNDE3MTE5._V1_SX300.jpg"},{"Title":"Frozen River","Year":"2008","imdbID":"tt0978759","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMTk2NjMwMDgzNF5BMl5BanBnXkFtZTcwMDY0NDY3MQ@@._V1_SX300.jpg"},{"Title":"Frozen Fever","Year":"2015","imdbID":"tt4007502","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMjY3YTk5MjUtODBjOC00NzAwLTgyYjYtMzFmMzAxOTZmOWRlXkEyXkFqcGdeQXVyNDgyODgxNjE@._V1_SX300.jpg"},{"Title":"Olaf's Frozen Adventure","Year":"2017","imdbID":"tt5452780","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BOWQ1NjNiZTEtYzc3Zi00Nzk4LTg5MTYtNzc5NmJjYTg1MGQ4XkEyXkFqcGdeQXVyMTA4NDI1NTQx._V1_SX300.jpg"},{"Title":"Frozen Stiff","Year":"2002","imdbID":"tt0301634","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMTk5NDc0MjU3Nl5BMl5BanBnXkFtZTcwNDc3NTU3OQ@@._V1_SX300.jpg"},{"Title":"Frozen Land","Year":"2005","imdbID":"tt0388318","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMTczMjgyMjQxNF5BMl5BanBnXkFtZTcwNjUxMjU3Mg@@._V1_SX300.jpg"},{"Title":"A Frozen Flower","Year":"2008","imdbID":"tt1155053","Type":"movie","Poster":"https://m.media-amazon.com/images/M/MV5BMWZhNjQ4ZTItNDM0NS00NGEzLWE5YjAtMmJmM2RhMTMwNzNkXkEyXkFqcGdeQXVyMzQ2MDUxMTg@._V1_SX300.jpg"}],"totalResults":"310","Response":"True"}
```

## Vuex(Store) Summery
src/components/MovieList.vue
- movies 배열에서 아이템을 이용할 수 있도록 v-for 이용하여 작성
```vue
<template>
	<div class="container">
		<div class="inner">
		<MovieItem
			v-for="movie in movies"
			:key="movie.imdbID" />
		</div>
	</div>
</template>

<script>
import MovieItem from '~/components/MovieItem';
export default {
	components: {
			MovieItem
		},
		data() {
			return {
			movies: []
		}
	}
}
</script>
```

src/components/MovieItem.vue
- 기본만 작성
```vue
<template>
	<div>
	</div>
</template>

<script>
export default {
}
</script>

<style lang="scss" scoped>
</style>
```

src/routes/Home.vue
- MovieList 연결
```vue
<template>
....
<MovieList />
</template>

<script>
import Headline from '~/components/Headline';
import Search from '~/components/Search';
import MovieList from '~/components/MovieList';
export default {
	components: {
		Headline,
		Search,
		MovieList
	}
}
</script>
```

### vuex
>형제 components 들은 어떻게 자원 공유할까?
부모에서 자식에게는 props를 사용
상하위에서는 Provide/ Inject를 활용
부모를 매개체로 삼아서 정보 전달이 가능하지만 멀어질수록 복잡해지므로
vuex를 이용하여 중앙집중식 상태 관리 라이브러리를 이용(store)

## Vuex(Store) Component
- vuex 설치
>npm i vuex@next

src/store/index.js
- 보조모듈 연결
```javascript
import { createStore } from 'vuex'
import { movie} from './movie'
import { about } from './about'

export default createStore({
	modules: {
		movie, // movie: movie
		about
	}
})
```

src/main.js
- store 연결
- index.js는 폴더명을 지정하면 자동으로 import
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './routes'
import store from './store'

createApp(App)
.use(router)
.use(store)
.mount('#app')
```

src/store/movie.js
- movie 모듈 작성
```javascript
export default {
	// module
	namespaced: true,
	// data!
	state: () => ({
		movies: []
	}),
	// computed : 계산된 상태
	getters: {
		movieIds(state) {
		return state.movies.map(m => m.imdbID)
		}
	},
	// methods와 유사
	// 변이
	// 여기에서만 데이터를 수정 가능
	mutations: {
		resetMovies(state) {
		state.movies = []
		}
	},
	// method 처럼 활용
	// 비동기로 동작
	actions: {
		searchMovies() {
		}
	}
}
```

- reference
[vuex components](https://vuex.vuejs.org/)

## Search Movie

### vuex의 js 수정
- state의 수정은 mutations에서만 가능
- 호출되는 부분(actions)에서 context의 commit을 이용하여 mutations 메소드를 호출하여 사용가능
- 하지만 state가 여러개일 경우 일일이 mutations에 만들어줘야 하는 불편함 존재
```javascript
import axios from 'axios';
export default {
	namespaced: true,
	state: () => ({
		movies: []
	}),
	getters: {
	},
	mutations: {
		assignMovies (state, Search) {
		state.movies = Search
	},
	resetMovies(state) {
		state.movies = []
	},
	actions: {
		async searchMovies(context, payload) {
		const {title, type, number, year} = payload
		const OMDB_API_KEY = '7035c60c'
		const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
		const { Search, totalResult} = res.data
		context.commit('assignMovies', Search)
	}}
}
```

src/store/index.js
- movie와 about 모듈을 분리하여 store로 연결
```javascript
import { createStore } from 'vuex'
import movie from './movie'
import about from './about'

export default createStore({
	modules: {
		movie, // movie: movie
		about
	}
})
```

src/store/movie.js
- 기존 Search.vue의 apply 메소드의 내용을 movie.js의 actions로 이동
- searchMovies를 위해 사용될 인수 정리 및  mutations의 updateState를 생성
```javascript
import axios from 'axios';
export default {
	// 현재 파일(movie.js)을 Store 모듈로 활용하려면 다음 옵션이 필요합니다.
	namespaced: true,
	// Vue.js data 옵션과 유사합니다.
	// 상태(State)는 함수로 만들어서 객체 데이터를 반환해야 가변 이슈(데이터 불변성)가 발생하지 않습니다!
	state: () => ({
		movies: [],
		message: '',
		loading: false
	}),
	// Vue.js computed 옵션과 유사합니다. 계산된 상태
	getters: {
		// movieIds(state) {
		// return state.movies.map(m => m.imdbID)
		// }
	},
	// Vue.js methods 옵션과 유사합니다.
	// 상태(State)는 변이(Mutations)를 통해서만 값을 바꿀 수 있습니다.
	// 변이
	// 여기에서만 데이터를 수정 가능
	mutations: {
		updateState(state, payload) {
			Object.keys(payload).forEach(key => {
			state[key] = payload[key];
		})
	},
		// assignMovies (state, Search) {
		// state.movies = Search
		// },
		resetMovies(state) {
			state.movies = []
		}
	},
	// Vue.js methods 옵션과 유사합니다.
	// 변이(Mutations)가 아닌 나머지 모든 로직을 관리합니다.
	// 비동기로 동작합니다.
	// 비동기로 동작
	actions: {
		async searchMovies({commit}, payload) {
			const {title, type, number, year} = payload
			const OMDB_API_KEY = '7035c60c'
			const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
			const { Search, totalResult} = res.data
			commit('updateState', {
				movies: Search
			})
		}
	}
}
```
src/components/Search.vue
- apply에는 store로부터 searchMovies를 실행 할 수 있도록 인자를 제공
```vue
<script>
....
	methods: {
		async apply() {
			// Search movies..
			this.$store.dispatch('movie/searchMovies', {
			title: this.title,
			type: this.type,
			number: this.number,
			year: this.year
			})
		}
	}
....
</script>
```

src/components/MovieList.vue
- movies 결과를 반환 받기위해 computed에 store로부터 movie의 movies를 반환
- MovieItem 컴포넌트에서 출력하기 위해 movie를 컴포넌트에 전달
```vue
<template>
	<div class="container">
		<div class="inner">
			<MovieItem
				v-for="movie in movies"
				:key="movie.imdbID"
				:movie="movie" />
		</div>
	</div>
</template>

<script>
import MovieItem from '~/components/MovieItem';

export default {
	components: {
		MovieItem
	},
	// data() {
	// return {
	// movies: []
	// }
	// }
	computed: {
		movies() {
			return this.$store.state.movie.movies
		}
	}
}
</script>

<style lang="scss" scoped>
</style>
```


src/components/MovieItem.vue
- 전달 받은 movie(객체)에서 title만 출력
```vue
<template>
	<div>
		{{ movie.Title }}
	</div>
</template>

<script>
export default {
	props: {
		movie: {
			type: Object,
			// default: function() { return {}}
			default: () => ({})
		}
	}
}
</script>

<style lang="scss" scoped>
</style>
```

## Search Movie additional
src/store/movie.js
- 검색 전체 개수를 page당 출력 개수로 나누기 위한 코드 작성
- number 를 10으로 나눈 수에 따라 page 요청 수를 조정하여 api 로 요청
- 결과는 updateState를 이용하여 전달
```javascript
....
export default {
	....
	actions: {
		async searchMovies({state, commit}, payload) {
			....
			console.log(totalResults) // 268
			console.log(typeof totalResults) // string
			const total = parseInt(totalResults, 10)
			const pageLength = Math.ceil(total / 10)
			// 추가 요청 전송
			if (pageLength > 1) {
				for (let page = 2; page <= pageLength; ++page) {
					if (page > (number / 10)) break
					const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`)
					const { Search } = res.data
					commit('updateState', {
						movies: [...state.movies, ...Search]
					})
				}
			}
		}
	}
}
```

## Remove duplicate ID from MovieList
### lodash
- 중복제거를 위해 사용하는 모듈
> npm i lodash
> Array의 uniqBy 활용

- reference
[lodash - array - uniqBy](https://lodash.com/docs/4.17.15#uniqBy)

src/store/movie.js
- Search의 정보를 imdbID 기준으로 중복제거를 하여 movies에 반환
```javascript
....
import _uniqBy from 'lodash/uniqBy'
....

export default {
	....
	actions: {
		async searchMovies({state, commit}, payload) {
			....
			const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
			const { Search, totalResults} = res.data
			commit('updateState', {
				movies: _uniqBy(Search, 'imdbID')
			})
			....
					commit('updateState', {
						movies: [
							...state.movies,
							..._uniqBy(Search, 'imdbID')
						]
					})
				}
			....
```

## Asynchrous - Callback & Promise object

### promise object
#### callback
- 함수에 인수로 사용하는 함수를 callback
- 어떤 것의 뒤에서 호출한다는 개념
- a 함수 로직 뒤에서 b를 실행
- callback 인자 전달 가능
```javascript
function a(callback) {
	const str = 'Hellow'
	setTimeout(() => {
		console.log('A')
		callback(str)
	}, 1000)
}
function b() {
	console.log('B')
}
a(function (event) {
	console.log(event)
	b()
})
```

#### callback 지옥
- 실행의 다음 순서의 보장하는 장점을 가지고 있지만 관리가 불편
```javascript
function a(callback) {
	setTimeout(() => {
		console.log('A')
		callback()
	}, 1000)
}

function b(callback) {
	setTimeout(() => {
		console.log('B')
		callback()
	}, 1000)
}

function c(callback) {
	setTimeout(() => {
		console.log('C')
		callback()
	}, 1000)
}

function d(callback) {
	setTimeout(() => {
		console.log('D')
		callback()
	}, 1000)
}

a(function () {
	b(function() {
		c(function() {
			d(function() {
				console.log('Done!')
			})
		})
	})
})
```

#### promise object
- 이런 단점을 보완할 수 있는 것
- resolve로 실행을 보장
- await는 resolve를 기다림
- resolve의 인수를 반환 가능
```javascript
// function a(callback) {
//   setTimeout(() => {
//     console.log('A')
//     callback()
//   }, 1000)
// }

function a() {
// promise: 약속의 객체!
	return new Promise(function(resolve){
		setTimeout(() => {
			console.log('A')
			resolve('hellow')
		}, 1000);
	})
}

function b() {
	console.log('B')
}

async function test() {
	const res = await a()
	console.log('res:', res)
	b()
}

test()
```
약속(Promise) : 다음 실행 순서의 보장
```javascript
function a() {
	return new Promise(function(resolve){
		setTimeout(() => {
			console.log('A')
			resolve('hellow A')
		}, 1000);
	})
}

function b() {
	return new Promise(function(resolve){
	setTimeout(() => {
			console.log('B')
			resolve('hellow B')
		}, 1000);
	})
}

function c() {
return new Promise(function(resolve){
	setTimeout(() => {
			console.log('C')
			resolve('hellow C')
		}, 1000);
	})
}

function d() {
return new Promise(function(resolve){
	setTimeout(() => {
			console.log('D')
			resolve('hellow D')
		}, 1000);
	})
}

async function test() {
	const h1 = await a()
	const h2 = await b()
	const h3 = await c()
	const h4 = await d()
	console.log('Done!')
	console.log(h1, h2, h3, h4)
}

test()
```

- reference
[Promise - javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## Asyncronous - Exception(then, catch, finally)
- async와 await 키워드는 ECMAScript 2017 (ES8)에서 등장
- ECMAScript 2015 (ES6)에서는 어떻게 처리? => promise의 method를 이용
#### then 이용
```javascript
function a() {
	return new Promise((resolve) => {
		setTimeout(() => {
			console.log('A')
			resolve()
		}, 1000);
	})
}

function test() {
	// const promise = a()
	//   promise.then(() => {
	//     console.log('B')
	//   })
	a().then(() => {
		console.log('B')
	})
}
test()
```

하지만... callback 지옥과 비슷한 모습으로...
```javascript
function a() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('A')
      resolve()
    }, 1000);
  })
}

function b() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('B')
      resolve()
    }, 1000);
  })
}

function c() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('C')
      resolve()
    }, 1000);
  })
}

function d() {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log('D')
      resolve()
    }, 1000);
  })
}

function test() {
  a().then(() => {
    b().then(() => {
      c().then(() => {
        d().then(() => {
          console.log('Done')
        })
      })
    })
  })
}

test()
```
코드를 정리할 수 있음
```javascript
function test() {
  a().then(() => {
    return b()
  }).then(() => {
    return c()
  }).then(() => {
    return d()
  }).then(() => {
    console.log('Done')
  })
}

test()
```
또는
```javascript
function test() {
  a()
    .then(() => b())
    .then(() => c())
    .then(() => d())
    .then(() => {
      console.log('Done')
    })
}

test()
```

#### reject
- resolve는 정상동작이고 정상적이지 않는 동작에 대해 reject와 catch를 활용
```javascript
function a(number) {
  return new Promise((resolve, reject) => {
    if (number > 4) reject()
    setTimeout(() => {
      console.log('A')
      resolve()
    }, 1000);
  })
}

function test() {
  a(7)
    .then(() => {
      console.log('Resolve!')
    })
    .catch(() => {
      console.log('Reject!')
    })
}

test()
```

#### finally
- then과 catch 와 상관 없이 무조건 실행
```javascript
function test() {
  a(2)
    .then(() => {
      console.log('Resolve!')
    })
    .catch(() => {
      console.log('Reject!')
    })
    .finally(() => {
      console.log('Done')
    })
}

test()
```
- async와 await를 이용할 경우?
```javascript
async function test() {
  try {
    await a(8)
    console.log('Resolve')
  } catch (error) {
    console.log('Reject')
  } finally {
    console.log('Done')
  }
}

test()
```

## Asyncronous - API example
#### axios cdn
- reference
[axios - cdn](https://cdnjs.com/libraries/axios) html에 연결

sample code:
```javascript
function fetchMovies(title) {
  return new Promise(async (resolve, reject) => {
    const OMDB_API_KEY = '7035c60c'
    const res = await axios.get(`https://omdbapi.com?apikey=${OMDB_API_KEY}&s=${title}`)
    // console.log(res)
    resolve(res)
  })
}

async function test() {
  const res = await fetchMovies('frozen')
  console.log(res)
}

test()
```

- resolve를 반환 후 fetchMovies를 실행했을때
```json
Promise (<pending)
	[[Prototype]]: Promise
	[[PromiseState]]: "fulfilled"
	[[PromiseResult]] : Object
....
```

- 정상적인 result를 위해
```javascript
....
async function test() {
	const res = await fetchMovies('frozen')
	console.log(res)
}
....
```

- 함수를 만들어 처리하면 관리측면에서 효율적임
```javascript
async function test() {
  const res = await fetchMovies('frozen')
  console.log(res)
}
test()

function hello() {
  fetchMovies('jobs')
    .then(res => console.log(res))
}
hello()
```

- Error 처리 로직으로 구현
```javascript
function fetchMovies(title) {
  return new Promise(async (resolve, reject) => {
    try {
      const OMDB_API_KEY = '7035c60cqqq'
      const res = await axios.get(`https://omdbapi.com?apikey=${OMDB_API_KEY}&s=${title}`)
      // console.log(res)
      resolve(res)
    } catch (error) {
      console.log(error.message)
      reject('TEST!?!?!??')
    }
  })
}

async function test() {
  try {
    const res = await fetchMovies('frozen')
    console.log(res)
  } catch (test) {
    console.log(test)
  }
}
test()

function hello() {
  fetchMovies('jobs')
    .then(res => console.log(res))
    .catch(test => { console.log(test) })
}
hello()
```

### pending, fulfilled, rejected
- Code 상에서의 Promise 상태
```javascript
function fetchMovies(title) {
  // 대기(pending): 이행하지도, 거부하지도 않은 초기 상태.
  const OMDB_API_KEY = '7035c60cqqq'
  return new Promise(async (resolve, reject) => {
    try {
      const res = await axios.get(`https://omdbapi.com?apikey=${OMDB_API_KEY}&s=${title}`)
      // 이행(fulfilled): 연산이 성공적으로 완료됨.
      resolve(res)
    } catch (error) {
      console.log(error.message)
      // 거부(rejected): 연산이 실패함.
      reject('TEST!?!?!??')
    }
  })
}
```


## Refactoring - Search Movie codes
src/store/movie.js
- axios.get을 유용하게 활용할 수 있도록 내부에서만 사용하는 \_fetchMovies() 함수 작성
- payload 인수 정리
- try - catch를 이용하여 에러 상황 대비
	- 활용되고 있는 omdb api의 자체 에러 또한 처리할 수 있도록 코드 구성
```javascript
export default {
	....
	actions: {
		async searchMovies({state, commit}, payload) {
      try {
        const res = await _fetchMovie({
          ...payload,
          page: 1
        })
        const { Search, totalResults} = res.data
        commit('updateState', {
          movies: _uniqBy(Search, 'imdbID')
        })
        ....
        if (pageLength > 1) { 
          for (let page = 2; page <= pageLength; ++page) {
            if (page > (payload.number / 10)) break
            const res = await _fetchMovie({
              ...payload,
              page
            })
            ....
          }
        }
      } catch (message) {
        commit('updateState', {
          movies: [],
          message
        })
      }
    }
	}
}

function _fetchMovie(payload) {
  const { title, year, page, type} = payload
  const OMDB_API_KEY = '7035c60c'
  const url =  `https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`
  // 에러 발생용
  // const url =  `https://www.omdbapi.com/?apikey=${OMDB_API_KEY}`
  return new Promise((resolve, reject) => {
    axios.get(url)
      .then(res => {
        // console.log(res)
        if (res.data.Error) {
          reject(res.data.Error)
        }
        resolve(res)
      })
      .catch(err => {
        reject(err.message)
      })
  })
}
```

src/components/MovieList.vue
- movie.js에서 catch 구문에서 처리되는 message를 반환
- omdb api 활용 시 발생되는 에러를 출력
```vue
<template>
	<div class="message">
		{{ message }}
	</div>
</template>
<script>
	computed: {
		movies() {
			return this.$store.state.movie.movies
		},
		message() {
			return this.$store.state.movie.message
		}
	}
</script>
```


## Movie Items - display
### 요건
- 영화 포스터
- 출시년도
- 영화제목
- 출시년도와 영화제목 뒤는 블러처리

src/components/MovieList.vue
- movies의 영역 안에 movie를 출력
- 해당 영역의 스타일을 지정(flex, wrap, center)
```vue
<template>
	<div class="container">
		<div class="inner">
			<div class="message">
			{{ message }}
			</div>
			<div class="movies">
				<MovieItem
					v-for="movie in movies"
					:key="movie.imdbID"
					:movie="movie" />
			</div>
		</div>
	</div>
</template>
....
<style lang="scss" scoped>
	.container {
		.movies {
			display: flex;
			flex-wrap: wrap;
			justify-content: center;
		}
	}
</style>
```

src/components/MovieItem.vue
- 요건에 따른 요소 구성
- 요건에 따른 스타일 지정
```vue
<template>
	<div
		:style="{backgroundImage: `url(${movie.Poster})`}"
		class="movie">
		<div class="info">
			<div class="year">
				{{ movie.Year }}
			</div>
			<div class="title">
				{{ movie.Title }}
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
	@import "~/scss/main";
	.movie {
		$width: 200px;
		width: $width;
		height: $width * 3/2;
		margin: 10px;
		border-radius: 4px;
		background-color: $gray-400;
		background-size: cover;
		overflow: hidden;
		position: relative;
		.info {
			background-color: rgba($black, .3);
			width: 100%;
			padding: 14px;
			font-size: 14px;
			text-align: center;
			position: absolute;
			left: 0;
			bottom: 0;
		}
	}
</style>
```


## Movie Items - ellipsis & blur
(텍스트 말줄임 표시와 배경 흐림 처리)

#### ellipsis
```css
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

- reference
[whitespace mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space)
[text-overflow mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/text-overflow)

#### blur - 다중 필터 이용가능
단점!!
firefox에서 사용 불가...
```css
backdrop-filter: blur(4px) grayscale();
```

- reference
[backdrop-filter mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/backdrop-filter)

src/components/MovieItem.vue
- info 영역 blur 처리
- title 말줄임 표시
- 마우스 올렸을 때 테두리 생성
```vue
<script lang="scss" scoped>
	.movie {
		....
		&:hover::after {
				content: "";
				position: absolute;
				top: 0;
				bottom: 0;
				left: 0;
				right: 0;
				border: 6px solid $primary;
			}
			.info {
				background-color: rgba($black, .3);
				width: 100%;
				padding: 14px;
				font-size: 14px;
				text-align: center;
				position: absolute;
				left: 0;
				bottom: 0;
				backdrop-filter: blur(10px);
			.year {
				color: $primary;
			}
			.title {
				color: $white;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}
		}
	}
</script>
```

## Customize Container width
Grid Example
[bootstrap container](https://getbootstrap.com/docs/5.2/layout/containers)

|   |  Extra small <576px  | Small ≥576px  | Medium ≥768px | Large ≥992px | X-Large ≥1200px | XX-Large ≥1400px  |
|---|---|---|---|---|---|---|
|.container|100%|540px|720px|960px|1140px|1320px|
|.container-sm|100%|540px|720px|960px|1140px|1320px|
|.container-md|100%|100%|720px|960px|1140px|1320px|
|.container-lg|100%|100%|100%|960px|1140px|1320px|
|.container-xl|100%|100%|100%|100%|1140px|1320px|
|.container-xxl100%|100%|100%|100%|100%|1320px|
|.container-fluid|100%|100%|100%|100%|100%|100%|

수정가능
```scss
$container-max-widths: (
  sm: 540px,
  md: 720px,
  lg: 960px,
  xl: 1140px,
  xxl: 1320px
);
```

src/scss/main.scss
- 6개의 item poster가 나오도록 최대 너비 수정
```scss
$primary: #FDC000;
$container-max-widths: (
	sm: 540px,
	md: 704px,
	lg: 924px,
	xl: 1140px,
	xxl: 1364px
);
....
```

src/components/MovieList.vue
- item list의 container에 스타일 적용
```vue
....
<style lang="scss" scoped>
	@import "~/scss/main";
	.container {
		margin-top: 30px;
		.inner {
			background-color: $gray-300;
			padding: 10px 0;
			border-radius: 4px;
		}
		.movies {
			display: flex;
			flex-wrap: wrap;
			justify-content: center;
		}
	}
</style>
```

## Output Error message & Loading Anime

src/components/MovieList.vue
- v-if문과 message를 이용하여 동작에 따른 message 출력 여부를 분리
- loading 애니메이션 추가
- 이에 따른 스타일 추가
- 결과가 없을 경우에 필요한 스타일 추가
```vue
<template>
	<div class="container">
		<div
			:class="{ 'no-result': !movies.length}"
			class="inner">
			<div
				v-if="loading"
				class="spinner-border text-primary">
			</div>
			<div
				v-if="message"
				class="message">
				{{ message }}
			</div>
			<div
				v-else
				class="movies">
				<MovieItem
					v-for="movie in movies"
					:key="movie.imdbID"
					:movie="movie" />
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
	@import "~/scss/main";
	
	.container {
		margin-top: 30px;
		.inner {
			....
			text-align: center;
			&.no-result {
			padding: 70px 0;
			}
		}
		.message {
			color: $gray-400;
			font-size: 20px;
		}
		....
	}
</style>
```

src/store/movie.js
- message 기본 문구 등록
- searchMovie 실행 시 message 및 loading 초기화 코드 작성
- 중복 방지를 위한 state.loading 조건문 작성
```javascript
export default {
	....
	state: () => ({
		movies: [],
		message: 'Search for the movie title!',
		loading: false
	}),
	actions: {
		async searchMovies({state, commit}, payload) {
			if (state.loading) return
			
			commit('updateState', {
				message: '',
				loading: true
			})
			....
			try {
			....
				} finally {
					commit('updateState', {
					loading: false
					})
				}
			}
		}
		....
}
```

### 부하 테스트
> 개발자 도구의 네트워크 탭에서 No throttling 을 다른 Presets로 바꾸어 인터넷 속도를 조절하여 원하는 동작을 확인이 가능

### 로딩 상태 나타내기
- reference
[bootstrap spinner](https://getbootstrap.com/docs/5.2/components/spinners/)

## Footer
src/components/Footer.vue
- Logo 컴포넌트 연결
- 날짜와 닉네임 하단 출력
- 스타일 적용
```vue
<template>
	<footer>
		<Logo/>
		<a
			href="https://github.com/zer0bi9"
			target="_blank">(c){{ new Date().getFullYear() }} ZER0BI9</a>
	</footer>
</template>

<script>
	import Logo from '~/components/Logo';
	
	export default {
		components: {
			Logo
		}
	}
</script>

<style lang="scss" scoped>
	footer {
		padding: 70px 0;
		text-align: center;
		opacity: .3;
		.logo {
			display: block;
			margin-bottom: 4px;
		}
	}
</style>
```

src/App.vue
- Footer 컴포넌트 연결(전 영역)
```vue
<template>
	<Header />
	<RouterView />
	<Footer />
</template>

<script>
import Header from '~/components/Header'
import Footer from '~/components/Footer'

export default {
	components : {
		Header,
		Footer
	}
}
</script>
```
