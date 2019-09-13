<template>
  <v-app>
    <v-app-bar app>
      <v-col
        cols="12"
        md="4"
      >
        <br />
        <v-text-field
          v-model="newNodeLabel"
          label="Flow name"
          required
        ></v-text-field>
      </v-col>
      <v-col
        cols="12"
        md="4"
      >
        <br />
        <v-text-field
          v-model="firstname"
          label="First name"
          required
        ></v-text-field>
      </v-col>
      <v-col
        cols="12"
        md="4"
      >
        <br />
        <v-btn
          @click="addNode"
          depressed
          small
          color="primary"
        >Primary</v-btn>
      </v-col>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content>
      <simple-flowchart
        :scene.sync="scene"
        @nodeClick="nodeClick"
        @nodeDelete="nodeDelete"
        @linkBreak="linkBreak"
        @linkAdded="linkAdded"
        @canvasClick="canvasClick"
        :height="800"
      ></simple-flowchart>
    </v-content>
  </v-app>
</template>
<script>
import SimpleFlowchart from "./components/SimpleFlowchart.vue";
export default {
  name: "app",
  components: {
    SimpleFlowchart
  },
  data() {
    return {
      scene: {
        centerX: 1024,
        centerY: 140,
        scale: 1,
        nodes: [
          {
            id: 1,
            x: -700,
            y: -69,
            type: "Action",
            label: "Mamed"
          },
          {
            id: 2,
            x: -357,
            y: 80,
            type: "Script",
            label: "Siroglan"
          },
          {
            id: 3,
            x: -557,
            y: 80,
            type: "Rule",
            label: "Cesaret"
          },
          {
            id: 4,
            x: -213,
            y: 80,
            type: "Rule",
            label: "Vaqif"
          }
        ],
        links: [
          {
            id: 1,
            from: 1, // node id the link start
            to: 2 // node id the link end
          },
          {
            id: 2,
            from: 1, // node id the link start
            to: 3 // node id the link end
          },
          {
            id: 3,
            from: 1, // node id the link start
            to: 4 // node id the link end
          },
          {
            id: 4,
            from: 2, // node id the link start
            to: 3 // node id the link end
          }
        ]
      },
      newNodeType: 0,
      newNodeLabel: "",
      nodeCategory: ["rule", "action", "script", "decision", "fork", "join"]
    };
  },
  methods: {
    canvasClick(e) {
      window.console.log("canvas Click, event:", e);
    },
    addNode() {
      let maxID = Math.max(
        0,
        ...this.scene.nodes.map(link => {
          return link.id;
        })
      );
      this.scene.nodes.push({
        id: maxID + 1,
        x: -400,
        y: 50,
        type: this.nodeCategory[this.newNodeType],
        label: this.newNodeLabel ? this.newNodeLabel : `test${maxID + 1}`
      });
    },
    nodeClick(id) {
      window.console.log("node click", id);
    },
    nodeDelete(id) {
      window.console.log("node delete", id);
    },
    linkBreak(id) {
      window.console.log("link break", id);
    },
    linkAdded(link) {
      window.console.log("new link added:", link);
    },
    showdata() {
      window.console.log(this.scene.nodes);
    }
  }
};
</script>