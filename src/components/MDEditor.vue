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
  name: "MDEditor",

  data() {
    return {
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
  <div class="mdeditor">
    <div class="markdown-container">
      <header class="header-markdown">
        <div class="main-heading">
          <h2 class="in-app-heading-s">MARKDOWN</h2>
        </div>
      </header>
      <div class="md-text-cont">
        <textarea class="markdown-text" v-model="markdownText"></textarea>
      </div>
    </div>

    <div class="preview-container">
      <header class="header-preview">
        <div class="main-heading">
          <h2 class="in-app-heading-s">PREVIEW</h2>
          <div class="preview-button-container">
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
      </header>
      <div class="html-preview-text" v-html="previewMD"></div>
    </div>
  </div>
</template>

<style lang="scss">
.mdeditor {
  display: flex;
  flex-direction: row;
}

.markdown-container {
  border-right: 1px solid $grey4;
}

.markdown-container .preview-container {
  max-width: 50%;
}

.main-heading {
  display: inline-flex;
  min-height: 3rem;
  background-color: $off-white;
  width: 100%;
}

.in-app-heading-s {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding-left: $margin-16;
  flex-grow: 1;
}

.markdown-text {
  font-size: 1rem;
  font-family: "Roboto", mono;
  min-width: 50vw;
  min-height: 150vh;
  padding: 0.64rem $margin-16;
  overflow: scroll;
  line-height: 1.7rem;
}

.preview-container {
  width: 100%;

  .preview-button-container {
    display: flex;
  }
}

.preview-button {
  background: none;
  border: none;
  margin-right: 1.14rem;
}

.html-preview-text {
  width: 100%;
  padding: 22px $margin-16;

  // seperate each child element
  & > * {
    margin-bottom: 1.43rem;
  }
}
</style>
