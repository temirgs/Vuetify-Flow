<template>
  <v-app>
    <v-app-bar app>
      <v-col cols="12" md="4">
        <br />
        <v-text-field
          v-model="newNodeLabel"
          label="Flow name"
          required
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="4">
        <br />
        <v-text-field
          v-model="newNodeUrl"
          label="Image url"
          required
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="4">
        <br />
        <v-btn @click="addNode" depressed small color="primary">Save</v-btn>
        <v-btn
          style="margin-left:15px"
          @click="showJson"
          depressed
          small
          color="blue"
          >Show</v-btn
        >
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
        :height="1300"
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
            x: -390,
            y: -69,
            url:
              "https://img.freepik.com/free-vector/abstract-dynamic-pattern-wallpaper-vector_53876-59131.jpg?size=338&ext=jpg",
            lable: "Mamed"
          },
          {
            id: 2,
            x: -390,
            y: 80,
            url:
              "https://img.freepik.com/free-vector/abstract-dynamic-pattern-wallpaper-vector_53876-59131.jpg?size=338&ext=jpg",
            lable: "Siroglan"
          },
          {
            id: 3,
            x: -557,
            y: 80,
            url:
              "https://img.freepik.com/free-vector/abstract-dynamic-pattern-wallpaper-vector_53876-59131.jpg?size=338&ext=jpg",
            lable: "Cesaret"
          },
          {
            id: 4,
            x: -213,
            y: 80,
            url:
              "https://img.freepik.com/free-vector/abstract-dynamic-pattern-wallpaper-vector_53876-59131.jpg?size=338&ext=jpg",
            lable: "Vaqif"
          }
        ],
        links: [
          {
            id: 1,
            from: 1, // node id the link start
            to: 2, // node id the link end
            label: "yes"
          },
          {
            id: 2,
            from: 1, // node id the link start
            to: 3, // node id the link end
            label: "no"
          },
          {
            id: 3,
            from: 1, // node id the link start
            to: 4, // node id the link end
            label: "yes"
          }
        ]
      },
      newNodeType: 0,
      newNodeLabel: "",
      newNodeUrl: ""
    };
  },
  methods: {
    canvasClick(e) {
      // window.console.log("canvas Click, event:", e);
    },
    showJson(e) {
      window.console.log(this.scene.nodes);
    },
    addNode(e) {
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
        lable: this.newNodeLabel ? this.newNodeLabel : `test${maxID + 1}`,
        url: this.newNodeUrl
      });
    },
    nodeClick(id) {
      // window.console.log("node click", id);
    },
    nodeDelete(id) {
      // window.console.log("node delete", id);
    },
    linkBreak(id) {
      // window.console.log("link break", id);
    },
    linkAdded(link) {
      // window.console.log("new link added:", link);
    }
  }
};
</script>
