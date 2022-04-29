<template>
  <div>
    <v-col id="my_related"></v-col>
  </div>
</template>

<script>
import axios from "axios";
import * as d3 from "d3";
export default {
  setup() {},
  data() {
    return {
      relatedTalks: [],
    };
  },
  methods: {
    genearteNetwork(edges) {
      let hierarchy = d3.stratify()(this.skills);
      const treeLayout = d3.tree().size([1280, 500]);
      hierarchy = treeLayout(hierarchy);
      this.svg = d3
        .select("svg")
        .attr("width", "100%")
        .attr("height", 500)
        .attr("cursor", "grab")
        .attr("position", "relative");
      this.skillTree = this.svg.append("g");
      const links = hierarchy.links();
      console.log(links);
      console.log(edges);
      const nodes = hierarchy.descendants();
      this.skillTree
        .selectAll("circle")
        .data(nodes)
        .enter()
        .append("circle")
        .attr("width", 20)
        .attr("x", (d) => d.x)
        .attr("y", (d) => d.y);
    },
  },
  mounted() {
    this.$store.state.curpage = "Find Your Topic";
    var vm = this;
    axios
      .get(
        "https://raw.githubusercontent.com/YufeiLinUlysses/CS573_Final_Project/zeng/pages/data/ted_talk.json"
      )
      .then(async function (response) {
        var data = response.data;
        for (var d in data) {
          var cur = data[d];
          if (cur["title"] == vm.$route.params.videotitle) {
            console.log(cur["url"]);
            console.log(cur["related_talks"]);
            var related = cur["related_talks"];
            var nodes = [
              {
                title: cur["title"],
                url: cur["url"],
              },
            ];
            for (let i = 0; i < related.length; i++) {
              nodes.push({
                title: related[i]["title"],
                url: related[i]["hero"],
              });
            }
            console.log(nodes);

            var edges = [];
            for (let i = 1; i < related.length + 1; i++) {
              var edge = {
                source: 0,
                target: i,
              };
              edges.push(edge);
            }
            console.log(edges);
            break;
          }
        }
      });
  },
};
</script>
