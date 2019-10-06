<template>
  <div class="picker">
    <button class="picker__toggle-button" @focus="onPickerToggleButton"></button>
    <div class="picker__wrapper" v-if="isPickerOpened" @keyup="onPickerKeyUp($event)">
      <div class="picker__emojis-filters filters">
        <a
          class="filters__link"
          :class="{active: currentFilter === index}"
          v-for="(group, index) in filtersEmoji"
          :key="group.name"
          :href="`#${group.shortname}`"
          @click="onFilterLinkClick(index, group.shortname)"
          v-html="group.icon"
        ></a>
      </div>
      <div class="picker__search-panel">
        <!-- <input type="text" placeholder="search" /> -->
        <template v-for="item in emojisGroupItems('modifier', modifier)">
          <button
            v-html="renderEmojiFromShortname(item)"
            :key="item"
            @click="onModifierButtonClick(item)"
          ></button>
        </template>
      </div>
      <div class="picker__emojis-list emojis-list" ref="list">
        <div
          class="emojis-list__group"
          v-for="group in filtersEmoji"
          :key="group.shortname"
          :id="group.shortname"
        >
          <div class="emojis-list__group-title">{{ group.name }}</div>
          <button
            v-for="item in emojisGroupItems(group.shortname, modifier)"
            class="emojis-list__button"
            v-html="renderEmojiFromShortname(item)"
            :key="item"
            @click="onEmojiButtonClick(item)"
          ></button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import sprite from "emoji-assets/sprites/joypixels-sprite-32.min.css";
import joypixels from "emoji-toolkit";

export default {
  data: function() {
    return {
      emojisList: joypixels.emojiList,
      modifier: ":tone1:",
      currentFilter: 0,
      isPickerOpened: false
    };
  },
  created() {
    joypixels.sprites = true;
  },
  computed: {
    emojisGroups() {
      const emojisGroups = new Set();
      for (let item in this.emojisList) {
        emojisGroups.add(this.emojisList[item].category);
      }
      return emojisGroups;
    },
    filtersEmoji() {
      const filters = [];
      const groupsArray = [...this.emojisGroups];
      const groupsWithoutModifiersAndRegions = groupsArray.slice(0, -2);
      groupsWithoutModifiersAndRegions.forEach(group => {
        switch (group) {
          case "flags":
            filters.push({
              name: "Flags",
              icon: this.renderEmojiFromShortname(":flag_gb:"),
              shortname: "flags"
            });
            break;
          case "people":
            filters.push({
              name: "Smileys & People",
              icon: this.renderEmojiFromShortname(":yum:"),
              shortname: "people"
            });
            break;
          case "activity":
            filters.push({
              name: "Activity",
              icon: this.renderEmojiFromShortname(":basketball:"),
              shortname: "activity"
            });
            break;
          case "objects":
            filters.push({
              name: "Objects",
              icon: this.renderEmojiFromShortname(":bulb:"),
              shortname: "objects"
            });
            break;
          case "symbols":
            filters.push({
              name: "Symbols",
              icon: this.renderEmojiFromShortname(":heartpulse:"),
              shortname: "symbols"
            });
            break;
          case "nature":
            filters.push({
              name: "Animals & Nature",
              icon: this.renderEmojiFromShortname(":hamster:"),
              shortname: "nature"
            });
            break;
          case "travel":
            filters.push({
              name: "Travel & Places",
              icon: this.renderEmojiFromShortname(":rocket:"),
              shortname: "travel"
            });
            break;
          case "food":
            filters.push({
              name: "Food & Drink",
              icon: this.renderEmojiFromShortname(":pizza:"),
              shortname: "food"
            });
            break;
        }
      });
      return filters;
    }
  },
  methods: {
    renderEmojiFromShortname(shortname) {
      return joypixels.shortnameToImage(shortname);
    },
    emojisGroupItems(group, modifier) {
      const emojisGroupItems = [];
      for (let item in this.emojisList) {
        if (this.emojisList[item].category === group) {
          emojisGroupItems.push(item);
        }
      }
      return emojisGroupItems.filter(item =>
        item.indexOf("tone") !== -1 && group !== "modifier"
          ? item.charAt(item.length - 2) ===
            modifier.charAt(modifier.length - 2)
          : item
      );
    },
    pasteElementAtCaret(element) {
      let sel, range;
      sel = window.getSelection();
      if (sel.getRangeAt && sel.rangeCount) {
        range = sel.getRangeAt(0);
        range.deleteContents();
        const el = document.createElement("div");
        el.innerHTML = element;
        let frag = document.createDocumentFragment(),
          node,
          lastNode;
        while ((node = el.firstChild)) {
          lastNode = frag.appendChild(node);
        }
        range.insertNode(frag);
        if (lastNode) {
          range = range.cloneRange();
          range.setStartAfter(lastNode);
          range.collapse(true);
          sel.removeAllRanges();
          sel.addRange(range);
          lastNode.innerHTML = "";
        }
      }
    },
    onModifierButtonClick(modifier) {
      this.modifier = modifier;
    },
    onEmojiButtonClick(emoji) {
      const element = this.renderEmojiFromShortname(emoji);
      this.pasteElementAtCaret(element);
    },
    onFilterLinkClick(index, href) {
      this.currentFilter = index;
    },
    onPickerToggleButton() {
      this.isPickerOpened = !this.isPickerOpened;
    },
    onPickerKeyUp(e) {
      const escapeCode = 27;
      if (e.keyCode === escapeCode) {
        this.isPickerOpened = false;
      }
    }
  }
};
</script>
<style scope>
.picker__toggle-button {
  width: 24px;
  height: 24px;
  opacity: 0.6;
  cursor: pointer;
  transition: opacity 0.3s ease-in-out;
}
.picker__wrapper {
  display: flex;
  flex-direction: column;
  background: #fff;
  position: absolute;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.32);
  border-radius: 5px;
  height: 276px;
  width: 318px;
  top: 30px;
  right: 0px;
  z-index: 90;
  transition: all 0.25s ease-in-out;
  filter: alpha(Opacity=0);
  user-select: none;
}
.picker__emojis-filters {
  flex-shrink: 0;
}
.filters {
  display: flex;
  height: 40px;
  padding: 0 7px;
  background: #f5f7f9;
}
.filters__link {
  display: flex;
  align-items: center;
  flex-shrink: 0;
  width: 32px;
  height: 100%;
  padding: 0 2px;
  margin: 0 1px;
}
.filters__link:not(.active):not(:hover) {
  filter: grayscale(1);
}
.filters__link.active {
  background-color: #fff;
}
.filters__link span {
  position: static;
  margin: 0;
}
.picker__emojis-list {
  overflow: auto;
  scroll-behavior: smooth;
  flex-grow: 1;
}
.emojis-list__group {
  padding: 7px;
}
.emojis-list__button {
  box-sizing: content-box;
  width: 32px;
  height: 32px;
  padding: 2px;
  margin: 1px;
  border: 0;
  background-color: transparent;
}
.emojis-list__button:hover {
  border-radius: 4px;
  background-color: #e4e4e4;
  cursor: pointer;
}
.emojis-list__button span {
  position: static;
  width: 100%;
  height: 100%;
  margin: 0;
}
.emojis-list__group-title {
  font-size: 13px;
  color: #b2b2b2;
  line-height: 20px;
  padding: 7px 0 7px 7px;
}
</style>