<template>
    <div class="post">
        <PostBanner 
            :name="post.name" 
            :headline="post.headline" 
            :summary="post.summary" 
            :image="post.mainImage"
            :imageDarkness="post.darkImage"
        />
        <div class="[ container-narrow pt-600 pb-600 ]">
            <SanityContent class="page-content [ flow ] [ space-base-em ]" :blocks="post.body" />
        </div>
        <div class="post__featuredPosts [ container-narrow flow ] [ bg-light-shade-dark pt-800 pb-800 space-600 ]" >
            <PostCard class="post__featuredPosts__item" v-for="item in post.featuredPosts" :key="item._id" :item="item" />
        </div>
    </div>
</template>

<script>
import { groq } from '@nuxtjs/sanity'
import { SanityContent } from '@nuxtjs/sanity/dist/components/sanity-content'
import PostCard from "@/components/PostCard";

export default {
    async asyncData({ params, $sanity }) {
        const query = groq`*[_type == "post" && slug.current == "${params.slug}"]{
            name,
            slug,
            headline,
            summary,
            mainImage,
            body,
            darkImage,
            featuredPosts[]->{
                name,
                slug,
                headline,
                summary,
                mainImage
            }
        }[0]`
        const post = await $sanity.fetch(query)
        return { post }
    },
    components: {SanityContent,PostCard},
    meta: {
		pageClasses: ['moooce-prices'],
	},
}
</script>
<style lang="scss">
@import './assets/scss/config';

$outputTokenCSS: false;
@import './node_modules/gorko/gorko.scss';

.post{

    &__featuredPosts{

        // &__item{
        //     display: grid;
        //     grid-template-columns: 1fr;
        //     gap: get-size('400');

        //     @include media-query('md'){
        //         grid-template-columns: 1fr 1fr;
        //         gap: 0;

        //         &:nth-child(even){
                    
        //             .image-wrapper{
        //                 order: 2;
        //             }

        //             .content{
        //                 order: 1;
        //             }
        //         }
        //     }
        // }
    }
}
</style>
