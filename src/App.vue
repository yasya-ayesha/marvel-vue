<template>
    <div id="app">

        <app-header :changeSearch="changeSearch" />

        <div class="container">
            <h1 class="pt-3 pb-3">
                Персонажи Marvel
            </h1>

            <h5 v-if="!character.length && !loading">
                К сожалению, поиск не дал результатов :(
            </h5>

            <app-modal :character="character[characterIndex]" />

            <spinner v-if="loading" />

            <div class="row">
                <div 
                    v-for="(el, index) in character"
                    :key="el.id"
                    class="card mb-3" 
                    style="max-width: 540px;"
                >
                    <div class="row g-0">
                        <div class="col-md-4">
                            <img 
                                :src="el.thumbnail" 
                                class="img-fluid rounded-start"     
                                :alt="'Изображение '+el.name"
                            >
                        </div>
                        <div class="col-md-8">
                            <div class="card-body">
                                <h5 class="card-title">
                                    {{el.name}}
                                </h5>
                                <!-- Button trigger modal -->
                                <button 
                                    type="button" 
                                    class="btn btn-primary" 
                                    data-bs-toggle="modal" 
                                    data-bs-target="#exampleModal"
                                    @click="characterIndex = index"
                                >
                                    Подробнее
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";

    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner,
        },
        data: () => ({
            loading: true,
            characters: [],
            characterIndex: 0,
            search: ''
        }),
        methods: {
            fetchCharacters() {
                fetch('https://netology-api-marvel.herokuapp.com/characters')
                    .then(response => response.json())
                    .then(data => {
                        this.characters = data;
                        this.loading = false;
                    })
                    .catch(e => console.error(e))
            },
            changeSearch(value) {
                this.search = value;
            }
        },
        computed: {
            character() {
                const {characters, search} = this;
                return characters.filter((character) => {
                    return character.name.toLowerCase().indexOf(search.toLowerCase()) !== -1;
                })
            }
        },
        mounted() {
            this.fetchCharacters();
        }
    }
</script>

<style>

</style>
