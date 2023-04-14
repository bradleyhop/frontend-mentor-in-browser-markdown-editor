<script>
// local components
import MDEditor from "./components/MDEditor.vue";

export default {
  components: {
    MDEditor,
  },

  data() {
    return {
      light: true, // light or dark theme
      menuOpen: false, // sidebar menu closed by default
    };
  },
};
</script>

<template>
  <div class="menu-container" :class="menuOpen ? 'show-menu' : ''" >
    <div class="menu-content">
      <h1 class="sidebar-title">MY DOCUMENTS</h1>
      <button class="new-document-btn">+ New Document</button>
      <!-- dark and light mode slider down here -->
    </div>
  </div>
  <!-- Sidebar start -->
  <!-- dynamically set app-header so that the sidebar doesn't force it to the button -->
  <div
    class="app-header"
    :style="{ 'min-width': !menuOpen ? '100%' : 'calc(100% - 250px)' }"
  >
    <nav class="app-nav">
      <!-- toggle menu open here -->
      <button @click="menuOpen = !menuOpen" class="menu-button">
        <span v-if="!menuOpen">
          <img
            class="hamburger-icon"
            src="@/assets/img/icon-menu.svg"
            alt="hamburger icon to expand navigation"
          />
        </span>
        <span v-else>
          <img
            class="close-menu-icon"
            src="@/assets/img/icon-close.svg"
            alt="x icon to close side bar menu"
          />
        </span>
      </button>
    </nav>
    <!-- Sidebar end -->

    <img src="@/assets/img/logo.svg" alt="MARKDOWN logo" class="app-logo" />

    <div class="menu-separator-container">
      <div class="menu-separator"></div>
    </div>

    <div class="document-menu-container">
      <img
        src="@/assets/img/icon-document.svg"
        alt="icon of document"
        class="document-icon"
      />
      <div class="document-name">Document Name</div>
      <div class="filename"></div>
    </div>

    <div class="save-del-container">
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
    </div>
  </div>

  <main>
    <MDEditor />
  </main>
</template>

<style lang="scss">
.menu-container {
  background-color: $black2;
  float: left;
  height: 100vh;
  z-index: 1;
  top: 0;
  left: 0;
  margin-left: 0px; // changes with .show-menu on opening menu
  overflow-x: hidden;
  position: sticky;
  transition: width 0.3s ease-in;
  width: 0px; // changes with .show-menu on opening menu

  .menu-content {
    margin-left: 1.71rem;
    margin-top: 1.71rem;
    white-space: nowrap;
  }

  .sidebar-title {
    font-family: $roboto-ff;
    font-size: 1rem;
    font-weight: 500;
    letter-spacing: 2px;
    line-height: 1.14rem;
    color: $grey2;
    margin-bottom: 1.71rem;
  }

  .new-document-btn {
    color: $white;
    border: none;
    border-radius: 4px;
    background-color: $dark-orange;
    font-family: $roboto-ff;
    font-size: 1.07rem;
    padding: 10px 42px;
    cursor: pointer;

    &:hover {
      background-color: $light-orange;
    }
  }
}

.app-header {
  background-color: $black3;
  display: inline-flex;
  align-items: center;
  min-width: calc(100% - 250px);

  .document-menu-container {
    display: inline-flex;
    flex-grow: 1;

    .document-icon {
      height: 1rem;
      margin-right: 1.14rem;
    }

    .document-name {
      color: $white;
      font-size: 13px;
      line-height: 15px;
      font-family: $roboto-ff;
    }
  }

  .app-nav {
    display: inline-flex;
    height: 5rem;

    .menu-button {
      width: 5rem;
      background-color: $black4;
      border: none;
      cursor: pointer;

      &:hover {
        background-color: $dark-orange;
      }
    }
  }

  .menu-separator-container {
    display: inline-flex;
    align-items: center;

    .menu-separator {
      width: 1px;
      height: 2.35rem;
      background-color: #979797;
      margin: 0 24px 0 30px;
    }
  }

  .save-del-container {
    .delete-icon {
      border: none;
      background: none;
      margin-right: 1.86rem;

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
      margin-right: $margin-16;

      &:hover {
        cursor: pointer;
        background-color: $light-orange;
      }

      .save-changes-cont {
        display: flex;
        align-items: center;
        padding: 0.59rem 0.94rem;
      }

      .save-changes-icon {
        height: 0.91rem;
        width: 0.91rem;
        margin-right: 0.5rem;
      }

      .save-changes-text {
        font-family: $roboto-ff;
        font-size: 0.88rem;
        line-height: 1.29rem;
        color: $white;
      }
    }
  }

  .app-logo {
    margin: 0 2rem;
    height: 11px;
  }
}

.show-menu {
  width: 250px;
  margin-left: 250px;
}
</style>
