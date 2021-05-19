<template>
<div>
    <PostBanner :headline="postLandingPage.headline" :summary="postLandingPage.summary" :image="postLandingPage.mainImage" />
    <div class="posts">
        <!-- <a v-bind:href="'/blog'+ post.slug.current" v-text="post.name" /> -->
        <!-- <nuxt-link :to="{name:'blog-slug' , params:{slug:post.slug.current} }" aria-label="Gå til forsiden">{{ post.name }}</nuxt-link> -->
        <div class=" [ container-narrow flow ] [ pt-800 pb-800 space-600 ]" >
            <PostCard v-for="item in postLandingPage.posts" :key="item._id" :item="item" />
        </div>
    </div>
</div>
</template>

<script>
import { groq } from '@nuxtjs/sanity'
import PostBanner from "@/components/PostBanner";
import PostCard from "@/components/PostCard";

export default {
    async asyncData({ $sanity }) {
        const query = groq`*[_type == "postLandingPage"]{
            headline,
            summary,
            mainImage,
            "posts": *[_type == "post"]{
                name,
                slug,
                headline,
                summary,
                mainImage
            }
        }[0]`
        const postLandingPage = await $sanity.fetch(query)
        return { postLandingPage }
    },
    components: {PostBanner,PostCard},
}
</script>
<style lang="scss" scoped>
// .post-intro{
//     position: relative;

//     img{
//         position: absolute;
//         top: 0;
//         left: 0;
//         width: 100%;
//         height: 100%;
//         object-fit: cover;
//         object-position: top center;
//     }

//     .content{
//         position: relative;
//         z-index: 10;
//     }
// }
</style>