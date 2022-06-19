<script>
import DOMPurify from "dompurify";
import { marked } from "marked";

export default {
  data() {
    return {
      light: true, // light or dark theme
      markdownText: "", // on init, will show default MD text
      htmlText: "", // html from MD
    };
  },

  beforeCreate() {
    // load in default MD text as supplied in our local json file
    fetch("./src/assets/data/data.json")
      .then((response) => response.json())
      .then((data) => {
        this.markdownText = data[1].content;
      })
      .catch(() => {
        console.log("error!!");
      });
  },

  computed: {
    // 'watches' textarea field for changes and updates html preview
    preview: function () {
      return DOMPurify.sanitize(
        marked(this.markdownText, {
          pendantic: false,
          gfm: true,
          breaks: true,
          smartLists: true,
          xhtml: false,
        })
      );
    },
  },
};
</script>

<template>
  <!-- Sidebar start -->

  <header class="app-header">
    <nav>
      <button>
        <img
          src="@/assets/img/icon-menu.svg"
          alt="hamburger icon to expand
      navigation"
        />
      </button>
      <h1>MY DOCUMENTS</h1>
      <button>New Document</button>

      <!-- dark and light mode slider down here -->
    </nav>

    <!-- Sidebar end -->

    <img src="@/assets/img/logo.svg" alt="MARKDOWN logo" class="app-logo" />

    <div class="document-name">Document Name</div>
    <div class="filename"></div>

    <button>
      <img
        src="@/assets/img/icon-delete.svg"
        alt="trash can icon to delete markdown text"
      />
    </button>
    <button>Save Changes</button>
  </header>

  <main>
    <header class="header-markdown">
      <h2>Markdown</h2>
      <textarea class="markdown-text" v-model="markdownText"></textarea>
    </header>

    <header class="header-preview">
      <h2>Preview</h2>
      <!-- on press, hide the markdown code and only show the preview -->
      <div class="html-preview-text" v-html="preview"></div>
      <button>
        <img
          src="@/assets/img/icon-hide-preview.svg"
          alt="icon of eye indicating markdown preview"
        />
      </button>
    </header>
  </main>
</template>

<style lang="scss">
.app-header {
  background-color: $black3;
}

.app-logo {
  height: 1.5rem;
  background-color: black;
}

.markdown-text {
  width: 100%;
  height: 50vh;
}
</style>
