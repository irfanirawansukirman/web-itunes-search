<template>
    <div>
        <h1>Results for {{$route.params.id}}</h1>
        <!-- {{$store.state.albums}} -->
        <div v-if="albumExist">
            <div v-for="(album, index) in albumData">
                <Card
                    :title="album.collectionCensoredName"
                    :image="album.artworkUrl100"
                    :artistName="album.artistName"
                    :url="album.artistViewUrl"
                    :color="picker(index)"
                />
            </div>
            <!-- {{albumData}} -->
        </div>
        <div v-else>
            <h1>Could Not Find Lala</h1>
        </div>
    </div>
</template>
<script>

import axios from 'axios';
import Card from '~/components/Card.vue';

export default {
    asyncData({params}) {
        return axios.get(`https://itunes.apple.com/search?term=${params.id}&entity=album`)
        .then((response) => {
            return {albumData: response.data.results}
        });
    },

    middleware: 'search',

    methods: {
        picker(index) {
            return index % 2 == 0 ? 'green' : 'blue';
        }
    },

    components: {
        Card
    },

    computed: {
        albumExist() {
            return this.albumData.length > 0;
        }
    }
}
</script>

