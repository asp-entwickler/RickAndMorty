<script lang="ts">
    import ChartDetails from './components/ChartDetails.vue'
    import * as ramapi from 'rickmortyapi';
    import type { Character, Location } from '@/Interfaces';
    import { defineComponent, type PropType } from 'vue';

    export default defineComponent({

        components: {
            ChartDetails
        },

        data(): {
            activeItem: Character | null;
            isMobile: boolean;
            activeItemIndex: number;
            chartItems: Character[],
            page: number,
            pages: number,
            genderFilterItems: Array<string>,
            statusFilterItems: Array<string>,
            genderFilterValue: string | null,
            statusFilterValue: string | null

        } {
            return {
                activeItem: null,
                isMobile: false,
                activeItemIndex: -1,
                chartItems: [],
                page: 1,
                pages: 1,
                genderFilterItems: ['Female', 'Male', 'Genderless', 'unknown'],
                statusFilterItems: ['Alive', 'Dead', 'unknown'],
                genderFilterValue: null,
                statusFilterValue: null
            }
        },

        setup(props: any) { },

        watch: {

            page(newVal: number, oldVal: number) {
                this.onPageChange(newVal);
            },
            statusFilterValue(newVal: string) {
                this.onStatusFilterChange(newVal);
            },
            genderFilterValue(newVal: string) {
                this.onGenderFilterChange(newVal);
            },
        },

        async mounted() {
            this.loadPage(1);
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
            },

            onChartClick(item: Character, index: number) {

                this.$data.activeItem = item;

                const episodesArray = item.episode.map((str) => {
                    const parts = str.split('/');
                    return parts[parts.length - 1];
                });

                this.$data.activeItem.episode = episodesArray;
                this.$data.activeItemIndex = item.id;
            },

            onPageChange(newPage: number) {
                this.loadPage(newPage);
            },

            onStatusFilterChange(filterValue: string) {
                this.loadPage(1);
            },

            onGenderFilterChange(filterValue: string) {
                this.loadPage(1);
            },

            async loadPage(pageNumber: number | undefined) {

                if (!pageNumber)
                    pageNumber = 1;

                let filterObj: { [k: string]: any } = {};
                filterObj.page = pageNumber;

                if (this.$data.statusFilterValue)
                    filterObj.status = this.$data.statusFilterValue;

                if (this.$data.genderFilterValue)
                    filterObj.gender = this.$data.genderFilterValue;

                const charResp = await ramapi.getCharacters(filterObj);
                this.$data.pages = charResp.data.info?.pages ?? 1;
                let charPageData = charResp.data.results;
                this.$data.chartItems = charPageData ?? [];
            },
        }
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

                            <v-row>
                                <!--Status-->
                                <v-col>
                                    <v-select :items="statusFilterItems"
                                              density="comfortable"
                                              clearable
                                              v-model="statusFilterValue"
                                              label="Status">
                                    </v-select>
                                </v-col>

                                <!--Gender-->
                                <v-col>
                                    <v-select :items="genderFilterItems"
                                              density="comfortable"
                                              clearable
                                              v-model="genderFilterValue"
                                              label="Gender">
                                    </v-select>
                                </v-col>
                            </v-row>

                            <v-card class="mx-auto">
                                <v-list :items="chartItems" style="max-height:70vh">
                                    <v-list-item id="itemCharacter"
                                                 v-for="(item, id) in chartItems" ma-0 pa-0
                                                 :key="item.id"
                                                 :value="item"
                                                 @click="onChartClick(item, id)"
                                                 clickable
                                                 lines="three"
                                                 density="compact"
                                                 :class="{ 'active-list-item': activeItemIndex === item.id }">

                                        <v-card id="itemCard"
                                                class="d-flex align-center">
                                            <v-avatar size="40" class="mr-4 ml-4">
                                                <img :src="item.image" style="max-height: 40px; max-width: 40px;" alt="avatar">
                                            </v-avatar>
                                            <v-card id="itemCardContent">
                                                <v-list-item-title v-text="item.name" class="mb-1"></v-list-item-title>
                                                <v-list-item-subtitle v-html="item.status + ' | ' + item.gender"></v-list-item-subtitle>
                                            </v-card>
                                        </v-card>
                                    </v-list-item>
                                    
                                </v-list>
                            </v-card>

                            <v-pagination v-model="page"
                                          prev-icon="mdi-menu-left"
                                          next-icon="mdi-menu-right"
                                          :length="pages"
                                          class="pt-3"
                                          theme="dark"
                                          rounded="1">
                            </v-pagination>

                        </div>  <!-- box -->

                    </v-col>

                    <!-- Details -->
                    <v-col :cols="isMobile ? 12 : 6">
                        <div class="box">
                            <div class="greetings">
                                <h1 class="green">Character Detail</h1>
                            </div>
                            <ChartDetails :character="activeItem" />
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
        border-radius: 50%;
        width: 48px;
        height: 48px;
    }

    .box {
        padding: 20px;
        text-align: left;
    }

    header {
        line-height: 1.5;
    }

    .logo {
        display: block;
        margin: 0 auto 2rem;
    }

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
