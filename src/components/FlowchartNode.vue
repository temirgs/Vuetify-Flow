<template>
  <div
    class="flowchart-node"
    :style="nodeStyle"
    @mousedown="handleMousedown"
    @mouseover="handleMouseOver"
    v-on:dblclick="(counter += 1), funcao()"
    @mouseleave="handleMouseLeave"
    v-bind:class="{ selected: options.selected === id }"
  >
    <div
      class="node-port node-input"
      @mousedown="inputMouseDown"
      @mouseup="inputMouseUp"
    ></div>
    <div class="node-main">
      <div v-text="label" class="node-type"></div>
      <div>
        <img style="height:69px; width:90px" :src="url" />
      </div>
    </div>
    <div class="node-port node-output" @mousedown="outputMouseDown"></div>
    <div v-show="show.delete" class="node-delete">&times;</div>
    <v-dialog v-model="dialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Profile</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  v-model="label"
                  label="Flow name"
                  type="text"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="URL"
                  label="Image url"
                  type="text"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <div class="flex-grow-1"></div>
          <v-btn color="blue darken-1" text @click="dialog = false"
            >Close</v-btn
          >
          <v-btn color="blue darken-1" text @click="SaveChange">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  name: "FlowchartNode",
  props: {
    id: {
      type: Number,
      default: 1000,
      validator(val) {
        return typeof val === "number";
      }
    },
    x: {
      type: Number,
      default: 0,
      validator(val) {
        return typeof val === "number";
      }
    },
    y: {
      type: Number,
      default: 0,
      validator(val) {
        return typeof val === "number";
      }
    },
    type: {
      type: String,
      default: "Default"
    },
    lable: {
      type: String
    },
    url: {
      type: String
    },
    options: {
      type: Object,
      default() {
        return {
          centerX: 1024,
          scale: 1,
          centerY: 140
        };
      }
    }
  },
  data() {
    return {
      dialog: false,
      counter: 0,
      show: {
        delete: false
      },
      label: this.lable,
      URL: this.url
    };
  },
  mounted() {},
  computed: {
    nodeStyle() {
      return {
        top: this.options.centerY + this.y * this.options.scale + "px", // remove: this.options.offsetTop +
        left: this.options.centerX + this.x * this.options.scale + "px", // remove: this.options.offsetLeft +
        transform: `scale(${this.options.scale})`
      };
    }
  },
  methods: {
    SaveChange() {
      this.dialog = false;
      this.$emit("itemchange", {
        url: this.URL,
        name: this.label,
        id: this.id
      });
    },
    funcao: function() {
      this.dialog = true;
    },
    handleMousedown(e) {
      const target = e.target || e.srcElement;
      if (
        target.className.indexOf("node-input") < 0 &&
        target.className.indexOf("node-output") < 0
      ) {
        this.$emit("nodeSelected", e);
      }
      e.preventDefault();
    },
    handleMouseOver() {
      this.show.delete = true;
    },
    handleMouseLeave() {
      this.show.delete = false;
    },
    outputMouseDown(e) {
      this.$emit("linkingStart");
      e.preventDefault();
    },
    inputMouseDown(e) {
      e.preventDefault();
    },
    inputMouseUp(e) {
      this.$emit("linkingStop");
      e.preventDefault();
    }
  }
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$themeColor: rgb(33, 150, 243);
$portSize: 15;
.flowchart-node {
  margin: 0;
  width: 90px;
  height: 90px;
  position: absolute;
  box-sizing: border-box;
  border: none;
  background: white;
  z-index: 1;
  opacity: 0.9;
  cursor: move;
  transform-origin: top left;
  .node-main {
    text-align: center;
    .node-type {
      background: $themeColor;
      color: rgb(235, 235, 235);
      font-size: 13px;
      padding: 0px;
    }
  }
  .node-port {
    position: absolute;
    width: #{$portSize}px;
    height: #{$portSize}px;
    left: 50%;
    transform: translate(-50%);
    border: 1px solid #ccc;
    border-radius: 100px;
    background: white;
    &:hover {
      background: $themeColor;
      border: 1px solid $themeColor;
    }
  }
  .node-input {
    top: #{-2 + $portSize/-2}px;
  }
  .node-output {
    bottom: #{-2 + $portSize/-2}px;
  }
  .node-delete {
    position: absolute;
    right: -6px;
    top: -6px;
    font-size: 12px;
    width: 20px;
    height: 20px;
    color: $themeColor;
    cursor: pointer;
    background: rgb(255, 253, 253);
    border: 1px solid black;
    border-radius: 100px;
    text-align: center;
    &:hover {
      color: rgb(33, 150, 243);
    }
  }
}
.selected {
  box-shadow: 0 0 0 2px $themeColor;
}
</style>
