<script lang="ts">
    import { defineComponent } from 'vue';
    import * as ramapi from 'rickmortyapi';

    export default defineComponent({
        //components: {
        //    ChartList
        //}

        props: {
            //msg: {
            //    readonly: false,
            //    default: '',
            //    type: String
            //},
        },

        data() {
            return {
                pageLength: 0
            }
        },

        setup(props) {

            //props.msg

        },

        async mounted() {
            console.log('Chart List Mounted');
            const charResp = await ramapi.getCharacters();
            let pages = charResp.data.info?.pages;
            let charPage = charResp.data.results;

            console.log('Characters on the Page: ' + charPage?.length);

            console.log('Characters List: ' + charResp);
        },


    })
</script>

<template>
    <div class="greetings">
        <h1 class="green">Rick and Morty Characters List</h1>
        <!--<h3>
          You’ve successfully created a project with
          <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
          <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
        </h3>-->
    </div>

    <v-card class="mx-auto"
            max-width="450">
        <v-toolbar color="cyan-lighten-1">
            <v-btn variant="text" icon="mdi-menu"></v-btn>

            <v-toolbar-title>Inbox</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn variant="text" icon="mdi-magnify"></v-btn>
        </v-toolbar>

        <v-list :items="items"
                item-props
                lines="three">
            <template v-slot:subtitle="{ subtitle }">
                <div v-html="subtitle"></div>
            </template>
        </v-list>
    </v-card>


</template>

<style scoped>
    h1 {
        font-weight: 500;
        font-size: 2.2rem;
        position: relative;
        top: -10px;
    }

    h3 {
        font-size: 1.2rem;
    }

    .greetings h1,
    .greetings h3 {
        text-align: center;
    }

    @media (min-width: 1024px) {
        .greetings h1,
        .greetings h3 {
            text-align: left;
        }
    }
</style>
