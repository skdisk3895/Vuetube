## 0603 Vue_pjt1 (props-emit, Youtube_API)

1. 컴포넌트 구조

   ```
   App.vue
   |
   |
   ---- MovieView.vue,
   |	|
   |	|
   |	-------- MovieList.vue
   |	|		|
   |	|		|
   |	|		-------------- MovieListItem.vue
   |   |
   |	-------- MovieListItemModal.vue
   |
   |
   ---- VideoView.vue
   	|
   	|
   	-------- VideoSearch.vue
   	|		
       -------- VideoItem.vue
       |
   	-------- VideoDetail.vue
   ```

2. 각 컴포넌트의 역할

   ### 1. 최상위 컴포넌트

   - `App.vue`

     가장 최상위 컴포넌트로, 모든 상황에 필요한 Navbar와 router-link를 넣었다.

     ```vue
     <template>
       <div class="container">
         <nav class="navbar navbar-light bg-light">
           <a class="navbar-brand text-info" href="#">
             <i class="fas fa-film"></i>
             영화저장소
           </a>
           <ul class="nav justify-content-center">
             <li class="nav-item">
               <router-link class="nav-link text-info" :to="{ name: 'MovieView' }">Movie</router-link>
             </li>
             <li class="nav-item">
               <router-link class="nav-link text-info" :to="{ name: 'VideoView' }">Video</router-link>
             </li>
           </ul>
         </nav>
         <div class="container">
           <router-view/>
         </div>
       </div>
     </template>
     
     <script>
     export default {
       name: 'App',
     }
     </script>
     
     <style>
     </style>
     ```

   

   ### 2. Movie관련 컴포넌트

   - `Movieview.vue`

     `movies` 가 비어있으면 (= 검색하기전 상태) 화면이 버튼이 보이게하고, 클릭(하위 컴포넌트(`MovieList.vue`)의 emit을 통한 데이터 전달)에 의해서 `movies` 안에 영화 data가 들어가지면 버튼을 감추고 영화 리스트가 나오게한다.

     ```vue
     <template>
       <div class="container mt-4 text-center">
           <h2>나만의 무비 컬렉션</h2>
           <MovieListItemModal :movie="movie" />
           <div v-if="movies">
             <MovieList class="mt-5" @movie-detail="getMovieDetail" :movies="movies" />
           </div>
           <div v-else>
             <button class="btn btn-info mt-2" @click="getMovies">영화 가져오기</button>
           </div>
       </div>
     </template>
     
     <script>
     import axios from 'axios'
     import MovieList from '@/components/MovieList.vue'
     import MovieListItemModal from '@/components/MovieListItemModal.vue'
     
     export default {
         name: 'MovieView',
         components: {
             MovieList,
             MovieListItemModal,
         },
         data() {
             return {
                 movies: '',
                 movie: {},
             }
         },
         methods: {
             getMovies() {
                 axios.get('http://www.json-generator.com/api/json/get/ceNyuXZmwi?indent=2')
                     .then(res => this.movies = res.data)
             },
             getMovieDetail(movie) {
                 this.movie = movie
             }
         },
         mounted() {
             
         }
     }
     </script>
     
     <style>
     </style>
     ```

   - `MovieList.vue`

     `props` 방식으로 상위 컴포넌트(`MovieView.vue`) 에 의해 전달받은 데이터를 이용해 각각 하나의 데이터(`movie`)를 하위 컴포넌트(`MovieListItem.vue`) 에게 `props` 방식으로 전달한다. 또한 `emit` 방식으로 하위 컴포넌트로부터 받은 데이터를 상위컴포넌트 (`MovieView.vue`) 로 `emit` 방식으로 전달한다.

     ```vue
     <template>
       <div class="container">
         <div class="row">
             <MovieListItem
                 @movie-detail="getMovieDetail"
                 v-for="movie in movies"
                 :key="movie.id"
                 :movie="movie"
             />
         </div>
       </div>
     </template>
     
     <script>
     import MovieListItem from '@/components/MovieListItem.vue'
     
     export default {
         name: 'MovieList',
         components: {
             MovieListItem
         },
         props: {
             movies: Array,
         },
         methods: {
             getMovieDetail(movie) {
                 // console.log(movie)
                 this.$emit('movie-detail', movie)
             }
         }
     }
     </script>
     
     <style>
     </style>
     ```

   - `MovieListItem.vue`
     `props` 방식과 `for`문을 통해 상위 컴포넌트(`MovieList.vue`)에서 받은 data를 웹페이지에 출력한다. `button` 태그안에 속성을 `data-toggle`  와 `data-target`을 넣음으로써 `modal`을 동작하게 한다. 또한 버튼을 누르면 `emit` 방식을 통해 해당 영화 데이터를 상위 컴포넌트(`MovieList.vue`)에 전달한다. 

     ```vue
     <template>
       <div class="col-3 mt-4">
         <div class="card" style="width:200px; height: 420px">
           <img :src="movie.poster_url" alt="" width="100%" height="300px">
           <div class="card-body">
             <h5 class="card-title" style="font-size: 18px;">{{ movie.title }}</h5>
             <button data-toggle="modal" data-target="#movieDetail" @click="getMovieDetail(movie)" class="btn btn-info mt-2" style="font-size: 12px;">영화 정보 상세보기</button>
           </div>
         </div>
       </div>
     </template>
     
     <script>
     export default {
         name: 'MovieListItem',
         props: {
           movie: Object
         },
         methods: {
           getMovieDetail(movie) {
             this.$emit('movie-detail', movie)
           }
         }
     }
     </script>
     
     <style>
     </style>
     ```

   - `MovieListItemModal.vue`

     (`button.data-toggle`, `modal.class`) 와 (`button.data-target`, `modal.id`) 값을 맞춤으로써 버튼을 클릭하면 `modal`창을 뜨게했다.`props` 방식으로 상위 컴포넌트(`MovieView.vue`) 로 부터 받은 데이터를 출력했다.
     
     ```vue
     <template>
       <div id="movieDetail" class="modal" tabindex="-1" role="dialog">
         <div class="modal-dialog">
           <div class="modal-content">
             <div class="modal-header">
               <h5 class="modal-title">{{ movie.title }}</h5>
               <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                 <span aria-hidden="true">&times;</span>
               </button>
             </div>
             <div class="modal-body">
               <img :src="movie.poster_url" alt="" width="100%" height="400px">
               <hr>
               <div>
                 <h2><span class="badge badge-success">{{ movie.user_rating }}</span></h2>
               </div>
               <p>{{ movie.overview }}</p>
             </div>
             <div class="modal-footer">
               <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
             </div>
           </div>
         </div>
       </div>
     </template>
     
     <script>
     export default {
         name: 'MovieListItemModal',
         props: {
           movie: Object
         }
     }
     </script>
     
     <style>
   </style>
     ```

    

   ### 3. Video 관련 컴포넌트

   - `VideoView.vue`

     `Youtube API` 와 `axios` 로 영상데이터를 가져온다. `git`에 `API_KEY` 가 노출되면 곤란해질 수 있기 때문에 `.env.local`에 key값을 저장하고 `.gitignore`에 `.env.local`  를 넣는다.  `axios.get` 할 때, `URL`을 직접 하드코딩하면 보기 안좋으므로 `params` 에 필요한 값을 넣는다. `video` 가 비어있으면 (영상리스트 중 하나의 영상을 클릭하지 않은 경우), 영상 검색 창과 영상 리스트를 보여주고, 그렇지 않으면 해당 영상만 웹페이지에 띄운다. 사실상 영상 관련 컴포넌트 중 최상위 컴포넌트라고 할 수 있다.

     ```vue
     <template>
       <div class="text-center mt-4">
           <div v-if="Object.keys(video).length">
             <VideoDetail :video="video" />   
           </div>
           <div v-else>
             <h2>영화 예고편 검색</h2>
             <VideoSearch @input-text="getInputValue" />
             <VideoItem  @video-detail="getVideoDetail" :videos="videos" />
           </div>
       </div>
     </template>
     
     <script>
     import axios from 'axios'
     import VideoSearch from '@/components/VideoSearch.vue'
     import VideoItem from '@/components/VideoItem.vue'
     import VideoDetail from '@/components/VideoDetail.vue'
     
     const API_KEY = process.env.VUE_APP_YOUTUBE_API_KEY
     const API_URL = 'https://www.googleapis.com/youtube/v3/search'
     
     export default {
         name: 'VideoView',
         components: {
             VideoSearch,
             VideoDetail,
             VideoItem,
         },
         data() {
             return {
                 inputValue: '',
                 videos: [],
                 video: {},
             }
         },
         Created () {
             console.log('create!!')
         },
         methods: {
             getInputValue(inputText) {
                 this.inputValue = inputText + ' trailer'
                 axios.get(API_URL, {
                     params: {
                         key: API_KEY,
                         part: 'snippet',
                         type: 'video',
                         q: this.inputValue
                     }
                 })
                     .then(res => this.videos = res.data.items)
             },
             getVideoDetail(movie) {
                 this.video = movie
             }
         }
     }
     </script>
     
     <style>
     </style>
     ```

   - `VideoSearch.vue`

     `emit` 방식으로 각각의 이벤트(`click`, `keypress.enter`)를 통해 상위 컴포넌트(`VideoView.vue`)에게 데이터를 전달한다.

     ```vue
     <template>
       <div class="mt-4 form-inline justify-content-center">
           <input class="form-control" @keypress.enter="onKeypressMoviePreview" v-model="inputText">
           <button class="btn btn-info" @click="onClickMoviePreview">검색</button>
       </div>
     </template>
     
     <script>
     export default {
         name: 'VideoSearch',
         data() {
             return {
                 inputText: ''
             }
         },
         methods: {
             onClickMoviePreview() {
                 this.$emit('input-text', this.inputText)
             },
             onKeypressMoviePreview() {
                 this.$emit('input-text', this.inputText)
             }
         },
     }
     </script>
     
     <style>
     </style>
     ```

   - `VideoItem.vue`

     `props` 방식을 통해 상위 컴포넌트(`VideoView.vue`)로부터 받은 데이터를 출력하고, `emit` 방식을 통해 상위 컴포넌트에게 데이터를 전달한다.

     ```vue
     <template>
       <div class="mt-5">
           <div @click="getMovieDetail(video)" class="mt-4 on-video" v-for="video in videos" :key="video.etag">
               {{ video.snippet.title }}
               <div>
                 <img :src="video.snippet.thumbnails.default.url" alt="" width="40%">
               </div>
           </div>
       </div>
     </template>
     
     <script>
     export default {
         name: 'VideoItem',
         props: {
             videos: Array
         },
         methods: {
             getMovieDetail(movie) {
                 this.$emit('video-detail', movie)
             }
         }
     }
     </script>
     
     <style>
     .on-video {
         cursor: pointer;
     }
     
     .on-video:hover {
         background-color: lightgrey;
     }
     </style>
     ```

   - `VideoDetail.vue`

     `props` 방식을 통해 상위 컴포넌트(`VideoView.vue`)로부터 받은 데이터를 이용해 영상에 대한 정보를 출력하고, `URL`은 `computed`속성을 통해 코드를 좀 더 간결하게 보이게 했다. 

     ```vue
     <template>
       <div>
         <div class="embed-responsive embed-responsive-16by9">
             <iframe class="embed-responsive-item" :src="videoUrl" allowfullscreen></iframe>
         </div>
         <h2>{{ video.snippet.title }}</h2>
       </div>
     </template>
     
     <script>
     export default {
         name: 'VideoDetail',
         props: {
             video: Object,
         },
         data() {
             return {
                 baseUrl: 'https://www.youtube.com/embed/'
             }
         },
         computed: {
             videoUrl() {
                 return this.baseUrl + this.video.id.videoId
             }
         }
     }
     </script>
     
     <style>
     </style>
     ```

     