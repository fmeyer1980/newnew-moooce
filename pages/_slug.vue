<template>
<div class="page">
    <PageHeader v-if="!pages.hidePageHeader" :pages="pages" />
    <div class="[ flow ] [ space-800 ]">
        <component v-for="item in pages.pageModules" :key="item._id" :is="componentMap[item._type]" :item="item" />
    </div>
    <nav class="sitebar-nav bg-light p-400 rounded-mid">
        <ul class="sitebar-nav__menu [ flow ] [ space-xxs ]">
            <li v-for="item in site.asideMenu" :key="item._key">
                <nuxt-link :to="{name:'slug',params:{slug:item.slug.current}}" :title="item.name">{{ item.name }}</nuxt-link>
            </li>
        </ul>
    </nav>
</div>
</template>

<script>
import {mapState} from 'vuex'
import { groq } from '@nuxtjs/sanity'

import PageHeader from '@/components/PageHeader'
import BodyText from '@/components/pageModules/BodyText'
import ImageTextbox from '@/components/pageModules/ImageTextbox'
import HowToUse from '@/components/pageModules/HowToUse'
import Intro from '@/components/pageModules/Intro'
import PriceList from '@/components/pageModules/PriceList'
import CoverImage from '@/components/pageModules/CoverImage'
import Youtube from '@/components/pageModules/Youtube'


export default {
    async asyncData({ params, $sanity }) {
        const query = groq`*[_type == "pages" && slug.current == "${params.slug}"]{
            name,
            slug,
            headline,
            summary,
            showSitebar,
            hidePageHeader,
            pageModules[]{
                _key,
                products[]->,
                ...,
            }
        }[0]`
        const pages = await $sanity.fetch(query)
        return { pages }
    },
    components: {PageHeader},
    computed:{
        ...mapState('sanity',{
            site: 'siteSettings'
        })
    },
    data(){
        return {
            componentMap: {
                bodyText: BodyText,
                imageTextBox: ImageTextbox,
                howToUse: HowToUse,
                intro: Intro,
                priceList: PriceList,
                coverImage: CoverImage,
                youtube: Youtube,
                youtube: Youtube
            }
        }
    }
}
</script>

<style lang="scss" scoped>

    @import './assets/scss/config';
    $outputTokenCSS: false;

    @import './node_modules/gorko/gorko.scss';

    .page{
        position: relative;

        .sitebar-nav{
            position: absolute;
            top: get-size('xl');
            right: get-size('container');
            z-index: 99;
            width: 20rem;
            display: none;

            a[aria-current]{
                @include apply-utility('weight','black')
            }

            @include media-query('lg'){
                display: block;
            }


        }
    }
</style>
