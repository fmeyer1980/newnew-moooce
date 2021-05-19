<template>
    <!-- <div class="bodyText-container" v-bind:class="(item.showSitebar)?'one-col':'two-col'"> -->
    <div class="container-narrow">
        <SanityBlocks class="page-content [ flow ] [ space-base-em ]" :blocks="item.body" :serializers="serializers" />
    </div>
</template>

<script>
    import { SanityBlocks } from 'sanity-blocks-vue-component';
    import ContentImage from '@/components/ContentImage'
	export default {
		name: "BodyText",
        components: {SanityBlocks},
        props:{
            item: Object
        },
        data() {
            return {
                serializers: {
                    types: {
                        image: ContentImage,
                    }
                }
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>