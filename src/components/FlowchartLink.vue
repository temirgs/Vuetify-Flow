<template>
  <g
    @mouseover="handleMouseOver"
    @mouseleave="handleMouseLeave"
    v-on:dblclick="(counter += 1), funcao()"
  >
    <!-- <text :transform="arrowTransform" font-size="15">true</text> -->
    <path :id="textId1" :d="dAttr" :style="pathStyle"></path>
    <a v-if="show.delete" @click="deleteLink">
      <text text-anchor="midlle" :transform="arrowTransform" font-size="27"
        >&times;</text
      >
    </a>
    <path
      :id="textId1"
      v-else
      d="M -1 -1 L 0 1 L 1 -1 z"
      :style="arrowStyle"
      :transform="arrowTransform"
    ></path>
    <!-- for lable -->
    <text x="50" y="60" style="text-anchor: start;">
      <textPath style="text-anchor: start;" :href="textId2">
        {{ conditionName }}
      </textPath>
    </text>
  </g>
</template>
<script>
export default {
  name: "FlowchartLink",
  props: {
    // start point position [x, y]
    start: {
      type: Array,
      default() {
        return [0, 0];
      }
    },
    // end point position [x, y]
    end: {
      type: Array,
      default() {
        return [0, 0];
      }
    },
    id: Number,
    lable: String
  },
  data() {
    return {
      show: {
        delete: false
      },
      textId1: "",
      textId2: "",
      counter: 0,
      startPoint: this.start,
      endPoint: this.end,
      conditionName: this.lable
    };
  },
  created() {
    this.textId1 = "MyPath" + this.id;
    this.textId2 = "#MyPath" + this.id;
  },
  methods: {
    funcao() {
      this.$emit("linkDblClick", {
        start: this.startPoint,
        end: this.endPoint,
        lable: this.conditionName,
        id: this.id
      });
    },
    handleMouseOver() {
      if (this.id) {
        this.show.delete = true;
      }
    },
    handleMouseLeave() {
      this.show.delete = false;
    },
    caculateCenterPoint() {
      // caculate arrow position: the center point between start and end
      const dx = (this.end[0] - this.start[0]) / 2;
      const dy = (this.end[1] - this.start[1]) / 2;
      return [this.start[0] + dx, this.start[1] + dy];
    },
    caculateRotation() {
      // caculate arrow rotation
      const angle = -Math.atan2(
        this.end[0] - this.start[0],
        this.end[1] - this.start[1]
      );
      const degree = (angle * 180) / Math.PI;
      return degree < 0 ? degree + 360 : degree; //360
    },
    deleteLink() {
      this.$emit("deleteLink");
    }
  },
  computed: {
    pathStyle() {
      return {
        stroke: "yellow",
        strokeWidth: 4,
        fill: "none"
      };
    },

    arrowStyle() {
      return {
        stroke: "red",
        strokeWidth: 10,
        fill: "none"
      };
    },
    arrowTransform() {
      const [arrowX, arrowY] = this.caculateCenterPoint();
      const degree = this.caculateRotation();
      return `translate(${arrowX}, ${arrowY}) rotate(${degree})`;
    },
    dAttr() {
      let cx = this.start[0],
        cy = this.start[1],
        ex = this.end[0],
        ey = this.end[1];
      let x1 = cx,
        y1 = cy + 50,
        x2 = ex,
        y2 = ey - 50;
      return `M ${cx}, ${cy} C ${x1}, ${y1}, ${x2}, ${y2}, ${ex}, ${ey}`;
    }
  }
};
</script>
<style scoped lang="scss">
g {
  cursor: pointer;
}
</style>
