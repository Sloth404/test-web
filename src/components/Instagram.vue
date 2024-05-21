<template>
    <div>
        <h1>Instagram Feed</h1>
        <div v-if="loading">Laden...</div>
        <div v-else>
            <div v-for="post in posts" :key="post.id">
                <img :src="post.media_url" :alt="post.caption">
                <p>{{ post.caption }}</p>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios';
import { InstagramPost } from '../types';

export default defineComponent({
    name: 'Instagram',
    setup() {
        const posts = ref<InstagramPost[]>([]);
        const loading = ref(true);

        const fetchInstagramData = async () => {
            try {
                const response = await axios.get('/instagram');
                posts.value = response.data.data;
            } cacth (error) {
                console.error('Fehler beim Abrufen der Instagram-Daten: ', error);
            } finally {
                loading.value = false;
            }
        };

        onMounted(() => {
            fetchInstagramData();
        });

        return {
            posts,
            loading
        };
    }
});
</script>

<style scoped>
img {
    width: 100%;
    height: auto;
}
</style>