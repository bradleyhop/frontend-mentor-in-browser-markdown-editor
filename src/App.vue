<script>
// marked will take the markdown and output html
import { marked } from "marked";
// remove any script nastiness that users may input
import DOMPurify from "dompurify";
// this sets the code highlighting to a certain colorscheme
// NOTE: will have to make our own based on design docs?
import "highlight.js/styles/github.css";
// add syntax highlighting to code
import hljs from "highlight.js/lib/common";

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
        console.error("fetch error!!");
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
          smartypants: true,
          xhtml: true,
          highlight: function (code) {
            console.log("code fired!");
            return hljs.highlightAuto(code).value;
          },
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
      <button>
        <img
          src="@/assets/img/icon-hide-preview.svg"
          alt="icon of eye indicating markdown preview toggle"
        />
      </button>
      <!-- HTML preview of MD -->
      <div class="html-preview-text" v-html="preview"></div>
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
  min-height: 50vh;
}
</style>
