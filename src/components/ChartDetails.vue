<script lang="ts">

    import { defineComponent, type PropType } from 'vue';
    import type { Character, Location } from '@/Interfaces';

    export default defineComponent({

        name: 'ChartDetail',
        components: {},

        props: {
            character: {
                type: Object as PropType<Character>,
                default: null
            }
        },

        mounted() {}
    });

</script>

<template>
    <v-card v-if="character">
        <v-img :src="character.image" height="300" width="300"></v-img>
        <v-card-title>{{ character.name }}</v-card-title>
        <v-card-subtitle> Status: {{ character.status }} | Species: {{ character.species }}  {{ character.type }}</v-card-subtitle>
        <v-card-text>
            <!--<div>ID: {{ character.id }}</div>-->
            <div>Gender: {{ character.gender }}</div>

            <div>Origin: <a :href="character.origin.url">{{ character.origin.name }}</a></div>
            <div>Location: <a :href="character.location.url">{{ character.location.name }}</a></div>

            <!--<div>Origin: {{ character.origin.name }}</div>
            <div>Location: {{ character.location.name }}</div>-->

            <div>
                Episodes:
                <span v-for="episode in character.episode" :key="episode">
                    <!--<a :href="episode">{{ episode }}</a>-->
                    {{ episode }}<span v-if="character.episode[character.episode.length - 1] != episode">, </span>
                </span>
            </div>
        </v-card-text>
        <!--<v-card-actions>
            <v-btn :href="character.url" text>Character Details</v-btn>
        </v-card-actions>-->
    </v-card>
</template>
