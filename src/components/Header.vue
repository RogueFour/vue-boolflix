.<template>
    <header class="bg-dark">
        <div class="row">
            <div class="col-12 ">
                <div class="row ">
                    <div class="col-6 d-flex justify-content-center logo">
                        <img src="../assets/img/Boolflix.png" alt="">
                    </div>
                    <div class="col-6 d-flex justify-content-center">
                        <div class="search d-flex align-items-center">
                            <input @keyup.enter="getMerge" type="text" placeholder="Search" v-model="inputSearch">
                            <button @click="getMerge">Cerca</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
</template>

<script>
import axios from 'axios';

export default { 
    name: 'Header',
    data() {
        return {
            inputSearch: '',
            list: {
                series: [],
                movies: [],
            },
            inputFixed: null,
            api_key: '401d2b8a5e51bdf6caf8b6e192d59d74',
            apiStatic: 'https://api.themoviedb.org/3/search/',
            language: 'en-US',       
        }
    },
    methods: {
        getFilm() {
            const endpoint = 'movie';
            const parameters = {
                api_key: this.api_key,
                language: this.language,
                query: this.inputSearch,
            };
            this.inputFixed = this.inputSearch.replace(/ /g,"+");
            axios.get(`${this.apiStatic}${endpoint}`, { params: parameters })
            .then((result) => {
                this.list.movies = result.data.results            
            })
            .catch((error) => {
                console.log(error);
            });
        },
        getSeries() {
            const endpoint = 'tv';
            const parameters = {
                api_key: this.api_key,
                language: this.language,
                query: this.inputSearch,
            };
            this.inputFixed = this.inputSearch.replace(/ /g,"+");
            axios.get(`${this.apiStatic}${endpoint}`, { params: parameters })
            .then((result) => {
                this.list.series = result.data.results
            })
            .catch((error) => {
                console.log(error);
            });
        },
        getMerge() {
            this.getFilm();
            this.getSeries();
            setTimeout(() => {
                this.$emit('doSearch', this.list)
            }, 500);
        }
    }
    
}
</script>
    
<style lang="scss">
    header {

    }
</style>