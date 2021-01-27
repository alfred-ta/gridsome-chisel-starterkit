<template>
  <Layout>
      <div class="flex flex-wrap items-start justify-start">

        <div class="order-2 w-full md:w-1/3 sm:pl-4 md:pl-6 lg:pl-8 sticky" style="top: 4rem">
        </div>

        <div class="order-1 w-full md:w-2/3" v-if="$page.allChiselDocument && $page.allChiselDocument.edges && $page.allChiselDocument.edges.length > 0">
          <h3>{{ $page.allChiselDocument.edges[0].node.Title }}</h3>
          <div class="content" v-html="$page.allChiselDocument.edges[0].node.Body" />
          <div class="mt-8 pt-8 lg:mt-12 lg:pt-12 border-t border-ui-border">
          </div>
        </div>

      </div>
  </Layout>
</template>

<page-query>
query ($Slug: String)  {
	allChiselDocument(filter: { Slug: { eq: $Slug } }){
    edges {
      node {
        id,
        title,
        Slug,
        Summary,
        Body
      }
    }
  }
}
</page-query>

<script>
import OnThisPage from '@/components/OnThisPage.vue';
import NextPrevLinks from '@/components/NextPrevLinks.vue';

export default {
  components: {
    OnThisPage,
    NextPrevLinks
  },
  mounted () {
    console.log(this.$route.params)
    console.log("this page", this.$page);
  },
  metaInfo() {
    const title = this.$page.allChiselDocument.edges[0].title;
    const summary = this.$page.allChiselDocument.edges[0].Summary;

    return {
      title: title,
      meta: [
        {
          name: 'description',
          content: summary
        },
        {
          key: 'og:title',
          name: 'og:title',
          content: title,
        },
        {
          key: 'twitter:title',
          name: 'twitter:title',
          content: title,
        },
        {
          key: 'og:description',
          name: 'og:description',
          content: summary,
        },
        {
          key: 'twitter:description',
          name: 'twitter:description',
          content: summary
        },
      ]
    }
  }
}
</script>

<style>
@import 'prism-themes/themes/prism-material-oceanic.css';
</style>