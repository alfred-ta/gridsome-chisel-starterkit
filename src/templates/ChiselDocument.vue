<template>
  <Layout>
      <div class="flex flex-wrap items-start justify-start">

        <div class="order-2 w-full md:w-1/3 sm:pl-4 md:pl-6 lg:pl-8 sticky" style="top: 4rem">
        </div>

        <div class="order-1 w-full md:w-2/3" v-if="$page.allChiselDocument && $page.allChiselDocument.edges && $page.allChiselDocument.edges.length > 0">
          <h1 :slug="$page.allChiselDocument.edges[0].node.Slug">{{ $page.allChiselDocument.edges[0].node.title }}</h1>
          <div class="content" v-html="convertedHTML" />          
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
  allChiselSection {
    edges {
      node {
        path
        title
        Documents {
          title
          Slug
        }
      }
    }
  }
}
</page-query>

<script>
import OnThisPage from '@/components/OnThisPage.vue';
import NextPrevLinks from '@/components/NextPrevLinks.vue';
import showdown from 'showdown';
import showdownHighlight from 'showdown-highlight';

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
  },
  computed: {
    convertedHTML() {
      const converter = new showdown.Converter({
        extensions: [showdownHighlight]
      });
      const md = this.$page.allChiselDocument.edges[0].node.Body;
      const html = converter.makeHtml(md);
      return html;
    }
  }
}
</script>

<style>
@import 'prism-themes/themes/prism-material-oceanic.css';
code[class*="language-"], pre[class*="language-"], code.hljs {
  display: block;
  padding: 1.25rem 1rem;
}
</style>