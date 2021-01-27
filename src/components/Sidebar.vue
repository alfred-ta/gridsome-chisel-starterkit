<template>
  <div
    ref="sidebar"
    v-if="showSidebar"
    class="px-4 pt-8 lg:pt-12"
  >
    <div
      v-for="({ node }, index) in sidebarSection"
      :key="node.path"
      class="pb-4 mb-4 border-ui-border"
      :class="{ 'border-b': index < sidebarSection.length -1 }"
    >
      <h3 class="pt-0 mt-0 mb-1 text-sm tracking-tight uppercase border-none">
        {{ node.title }}
      </h3>

      <ul class="max-w-full pl-2 mb-0">
        <li
          v-for="document in node.Documents"
          :id="document.Slug"
          :key="document.Slug"
          :class="getClassesForAnchor(document)"
          @mousedown="$emit('navigate')"
        >
          <g-link
            :to="`/documents/${document.Slug}`"
            class="flex items-center py-1 font-semibold"
          >
           <span
              class="absolute w-2 h-2 -ml-3 rounded-full opacity-0 bg-ui-primary transition transform scale-0 origin-center"
              :class="{
                'opacity-100 scale-100': currentDocument.Slug === document.Slug
              }"
            ></span>
            {{ document.title }}
          </g-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<static-query>
query Sidebar {
  metadata {
    settings {
      sidebar {
        name
        sections {
          title
          items
        }
      }
    }
  }
}
</static-query>

<script>
export default {
  data() {
    return {
      expanded: []
    };
  },
  computed: {
    pages() {
      return this.$page.allMarkdownPage.edges.map(edge => edge.node);
    },
    sidebarSection() {
      console.log("we mean it", this.$page);
      return this.$page.allChiselSection.edges;
      return null;
      return this.$static.metadata.settings.sidebar.find(
        sidebar => sidebar.name === this.$page.markdownPage.sidebar
      );
    },
    showSidebar() {
      return true; // default show for now
    },
    currentDocument() {
      return this.$page.allChiselDocument.edges[0].node;
    }
  },
  methods: {
    getClassesForAnchor({ slug }) {
      return {
        "text-ui-primary": this.currentDocument.Slug === slug,
        "transition transform hover:translate-x-1 hover:text-ui-primary": ! this.currentDocument.slug === slug
      };
    }
  },  
};
</script>
