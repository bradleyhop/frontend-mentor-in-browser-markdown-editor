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
    <nav class="app-nav">
      <button class="menu-button">
        <img
          class="hamburger-icon"
          src="@/assets/img/icon-menu.svg"
          alt="hamburger icon to expand
      navigation"
        />
      </button>
      <div class="menu-content">
        <h1>MY DOCUMENTS</h1>
        <button>New Document</button>

        <!-- dark and light mode slider down here -->
      </div>
    </nav>

    <!-- Sidebar end -->

    <img src="@/assets/img/logo.svg" alt="MARKDOWN logo" class="app-logo" />

    <div class="document-container">
      <img
        src="@/assets/img/icon-document.svg"
        alt="icon of document"
        class="document-icon"
      />
      <div class="document-name">Document Name</div>
      <div class="filename"></div>
    </div>

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

  .app-nav {
    display: inline-flex;
    height: 5rem;

    .menu-button {
      width: 5rem;
      background: none;
      border: none;
      cursor: pointer;

      .hamburger-icon {
        filter: invert(52%) sepia(100%) saturate(2%) hue-rotate(348deg)
          brightness(96%) contrast(94%);
      }
    }

    .menu-content {
      display: none;
    }
  }

  .app-logo {
    margin: 0 2rem;
    height: 11px;
  }
}

.document-container {
  display: inline-flex;

  .document-icon {
    height: 1rem;
  }

  .document-name {
    color: $white;
  }
}

.markdown-text {
  width: 100%;
  min-height: 50vh;
}

.html-preview-text {
  margin: 5rem;
}
</style>
