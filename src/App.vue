<script lang="ts">
    import HelloWorld from './components/HelloWorld.vue'
    import ChartList from './components/ChartList.vue'
    import TheWelcome from './components/TheWelcome.vue'

    import * as ramapi from 'rickmortyapi';

    import { defineComponent, type PropType } from 'vue';

    export default defineComponent({


        data() {
            return {
                isMobile: false,
                selectedItem: null,
                activeItem: false,
                activeItemIndex: -1,
                //chartItems: [] as PropType<ramapi.Character[]>
                chartItems: [] as ramapi.Character[]

                    //{
                    //    idx: 1,
                    //    prependAvatar: 'https://cdn.vuetifyjs.com/images/lists/1.jpg',
                    //    title: 'Brunch this weekend?',
                    //    subtitle: `<span class="text-primary">Ali Connors</span> &mdash; I'll be in your neighborhood doing errands this weekend. Do you want to hang out?`,
                    //},

                    //{
                    //    idx: 2,
                    //    prependAvatar: 'https://cdn.vuetifyjs.com/images/lists/2.jpg',
                    //    title: 'Summer BBQ',
                    //    subtitle: `<span class="text-primary">to Alex, Scott, Jennifer</span> &mdash; Wish I could come, but I'm out of town this weekend.`,
                    //},

                //]
            }
        },

        components: {
            ChartList,
            HelloWorld,
            TheWelcome

        },

        setup(props) {

            //this.chartItems

        },

        async mounted() {
            console.log('Chart List Mounted');
            const charResp = await ramapi.getCharacters();
            let pages = charResp.data.info?.pages;
            let charPage = charResp.data.results;
            this.$data.chartItems = charPage!;



            console.log('Characters on the Page: ' + charPage?.length);

            console.log('Characters List: ' + charResp);

            //this.$data.chartItems = charPage;


            //this.$data.chartItems.splice(0, this.$data.chartItems.length, ...charPage);
        },

        created() {
            this.checkMobile();
            window.addEventListener("resize", this.checkMobile);

        },
        beforeDestroy() {
            window.removeEventListener("resize", this.checkMobile);
        },

        methods: {

            checkMobile() {
                this.isMobile = window.innerWidth < 1024;
                //console.log('isMobile: ' + this.isMobile);
            },
            onChartClick(item: any, index: number) {

                this.$data.activeItemIndex = index;
                console.log("Chart Idx: " + item);

            }
        },

    })


</script>

<template>
    <v-app>

        <v-main>

            <v-container fluid ma-0 pa-0>

                <v-row style="border: 1px solid blue; min-width: 250px;">

                    <v-col :cols="isMobile ? 12 : 6">

                        <div class="box">

                            <div class="greetings">
                                <h1 class="green">Rick and Morty Characters List</h1>
                            </div>

                            <v-card class="mx-auto">

                                <v-list :items="chartItems" style="max-height:50vh">
                                    <v-list-item id="itemCharacter"
                                                 v-for="(item, id) in chartItems" ma-0 pa-0
                                                 :key="id"
                                                 :value="item"
                                                 @click="onChartClick(item, id)"
                                                 clickable
                                                 lines="three"
                                                 density="compact"
                                                 :class="{ 'active-list-item': activeItemIndex === id }">
                                            
                                            <v-card id="itemCard" 
                                                    class="d-flex align-center">
                                                <v-avatar size="40" class="mr-4 ml-4">
                                                    <img :src="item.image" style="max-height: 40px; max-width: 40px;" alt="avatar">
                                                </v-avatar>
                                                <v-card id="itemCardContent">
                                                    <v-list-item-title v-text="item.name" class="mb-1"></v-list-item-title>
                                                    <v-list-item-subtitle v-html="item.status"></v-list-item-subtitle>
                                                </v-card>
                                            </v-card>
                                        </v-list-item>
                                </v-list>

                            </v-card>

                        </div>

                    </v-col>

                    <!-- Details -->
                    <v-col :cols="isMobile ? 12 : 6">
                        <div class="box">
                            <TheWelcome />
                        </div>
                    </v-col>
                </v-row>
            </v-container>


        </v-main>

    </v-app>
</template>

<style scoped>

    #itemCharacter.active-list-item {
        background-color: #444444;
    }

    #itemCardContent, #itemCard, #itemCardContent:hover, #itemCard:hover {
        background-color: inherit;
    }

    .avatar img {
        border-radius: 50%; /* Make the image rounded */
        width: 48px; /* Adjust the width as needed */
        height: 48px; /* Adjust the height as needed */
    }

    .box {
        padding: 20px;
        text-align: left;
        /*border: 1px solid gray;*/
    }

    header {
        line-height: 1.5;
    }

    .logo {
        display: block;
        margin: 0 auto 2rem;
    }

    /*    @media (min-width: 1024px) {
        header {
            display: flex;
            place-items: start;
            padding-right: calc(var(--section-gap) / 2);
        }

        .logo {
            margin: 0 2rem 0 0;
        }

        header .wrapper {
            display: flex;
            place-items: flex-start;
            flex-wrap: wrap;
        }
    }*/


    #app > div {
        width: 100% !important;
    }


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
            text-align: center;
        }
    }
</style>
