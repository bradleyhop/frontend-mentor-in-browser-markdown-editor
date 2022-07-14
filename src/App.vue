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
        this.markdownText = "# Cannot find default Markdown text!!";
      });
  },

  computed: {
    // 'watches' textarea field for changes and updates html preview
    previewMD: function () {
      return DOMPurify.sanitize(
        marked(this.markdownText, {
          pendantic: false,
          gfm: true,
          breaks: true,
          smartLists: true,
          smartypants: true,
          xhtml: true,
          highlight: function (code) {
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

    <div class="menu-separator"></div>

    <div class="document-container">
      <img
        src="@/assets/img/icon-document.svg"
        alt="icon of document"
        class="document-icon"
      />
      <div class="document-name">Document Name</div>
      <div class="filename"></div>
    </div>

    <button class="delete-icon">
      <img
        src="@/assets/img/icon-delete.svg"
        alt="trash can icon to delete markdown text"
      />
    </button>
    <button class="save-changes-button">
      <span class="save-changes-cont">
        <img
          src="@/assets/img/icon-save.svg"
          alt="save icon"
          class="save-changes-icon"
        />
        <span class="save-changes-text">Save Changes</span>
      </span>
    </button>
  </header>

  <main>
    <header class="header-markdown">
      <div class="main-heading">
        <h2 class="in-app-heading-s">MARKDOWN</h2>
      </div>
      <div class="md-text-cont">
        <textarea class="markdown-text" v-model="markdownText"></textarea>
      </div>
    </header>

    <header class="header-preview">
      <div class="main-heading">
        <h2 class="in-app-heading-s">PREVIEW</h2>
        <div class="p-btn-cont">
          <button class="preview-button">
            <img
              src="@/assets/img/icon-hide-preview.svg"
              alt="icon of eye indicating markdown preview toggle"
            />
          </button>
        </div>
      </div>
      <!-- on press, hide the markdown code and only show the preview -->
      <!-- HTML preview of MD -->
      <div class="html-preview-text" v-html="previewMD"></div>
    </header>
  </main>
</template>

<style lang="scss">
$margin-16: 1.15rem; // offset for app elements

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

  .menu-separator {
    display: inline-block;
    width: 1px;
    height: 2.35rem;
    background-color: #979797;
    margin: 16px 24px 16px 30px;
  }

  .delete-icon {
    border: none;
    background: none;

    &:hover {
      filter: invert(53%) sepia(7%) saturate(7495%) hue-rotate(331deg)
        brightness(94%) contrast(90%);
      cursor: pointer;
    }
  }

  .save-changes-button {
    border: none;
    border-radius: 4px;
    background-color: $dark-orange;

    &:hover {
      cursor: pointer;
      background-color: $light-orange;
    }

    .save-changes-cont {
      display: flex;
      justify-content: center;
      padding: 0.59rem 0.94rem;
    }

    .save-changes-icon {
      height: 0.91rem;
      width: 0.91rem;
      margin-right: 0.5rem;
    }

    .save-changes-text {
      font-family: "Roboto", mono;
      font-size: 0.88rem;
      color: $white;
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

.main-heading {
  display: inline-flex;
  min-height: 3rem;
  background-color: $off-white;
  width: 100%;

  .in-app-heading-s {
    display: flex;
    justify-content: center;
    flex-direction: column;
    padding-left: $margin-16;
  }
}

.markdown-text {
  width: 100%;
  min-height: 50vh;
  padding: 0 $margin-16;
}

.html-preview-text {
  width: 100%;
  padding: 0 $margin-16;

  // seperate each child element
  & > * {
    margin-bottom: 1.43rem;
  }
}
</style>
