<template>
  <div class="home">
    <v-row class="mx-5 my-5">
      <v-text-field
        hide-details
        single-line
        v-model="searchedTopic"
        @keyup="checkForEnterKey"
      ></v-text-field>
      <v-btn text @click="getTopic()"><v-icon>mdi-magnify</v-icon></v-btn>
    </v-row>
    <v-col id="my_topics"></v-col>
  </div>
</template>

<script>
import axios from "axios";
import * as d3 from "d3";
export default {
  name: "Home",
  data() {
    return {
      topics: [],
      topicData: [],
      searchedTopic: "",
    };
  },
  methods: {
    getNums() {
      var number = [];
      for (var i = 0; i < 20; i++) {
        var flag = true;
        var num = Math.floor(Math.random() * 415);
        while (number.includes(num)) {
          num = Math.floor(Math.random() * 415);
        }
        if (flag == true) {
          number.push(num);
        }
      }
      return number;
    },
    gotoTopic(topic) {
      this.$router.push({
        path: `/topic/${topic}`,
      });
    },
    getTopic() {
      if (this.topics.includes(this.searchedTopic)) {
        var dataset = {
          children: [
            {
              Name: this.searchedTopic,
              Count: this.topicData[this.searchedTopic],
            },
          ],
        };
        d3.selectAll("svg").remove();
        this.generateBubble(dataset);
      } else {
        alert("Not Found");
      }
    },
    checkForEnterKey(event) {
      if (event.keyCode == 13) {
        alert(this.searchedTopic);
        this.getTopic();
      }
    },
    generateBubble(dataset) {
      var width = 600;
      var height = 600;
      var color = d3
        .scaleOrdinal()
        .range([
          "red",
          "green",
          "blue",
          "#6b486b",
          "#a05d56",
          "#d0743c",
          "#ff8c00",
        ]);

      var bubble = d3.pack(dataset).size([width, height]).padding(1.5);

      var svg = d3
        .select("#my_topics")
        .append("svg")
        .attr("preserveAspectRatio", "xMidYMid meet")
        .attr("viewBox", "-100 -40 1000 1000")
        .attr("class", "bubble");

      var nodes = d3.hierarchy(dataset).sum(function (d) {
        return d.Count;
      });

      var node = svg
        .selectAll(".node")
        .data(bubble(nodes).descendants())
        .enter()
        .filter(function (d) {
          return !d.children;
        })
        .append("g")
        .attr("class", "node")
        .attr("transform", function (d) {
          return "translate(" + d.x + "," + d.y + ")";
        });

      node.append("title").text(function (d) {
        return d.Name + ": " + d.Count;
      });

      node
        .append("circle")
        .attr("r", function (d) {
          return d.r;
        })
        .style("fill", function (d, i) {
          return color(i);
        })
        .on("click", function (event, d) {
          // click(d);
          console.log("click");
          console.log(d.data);
          var topic = d.data.Name;
          console.log(topic);
          //$.cookie('topic', topic)
          localStorage.setItem("topic", topic);
          window.open("/topic/" + topic, "_self");
        });

      node
        .append("text")
        .attr("dy", ".2em")
        .style("text-anchor", "middle")
        .text(function (d) {
          return d.data.Name.substring(0, d.r / 3);
        })
        .attr("font-family", "sans-serif")
        .attr("font-size", function (d) {
          return d.r / 5;
        })
        .attr("fill", "white");

      node
        .append("text")
        .attr("dy", "1.3em")
        .style("text-anchor", "middle")
        .text(function (d) {
          return d.data.Count;
        })
        .attr("font-family", "Gill Sans", "Gill Sans MT")
        .attr("font-size", function (d) {
          return d.r / 5;
        })
        .attr("fill", "white");

      d3.select(self.frameElement).style("height", height + "px");
    },
  },
  mounted() {
    this.$store.state.curpage = "Find Your Topic";
    var vm = this;
    axios
      .get(
        "https://raw.githubusercontent.com/YufeiLinUlysses/CS573_Final_Project/zeng/pages/data/tags.json"
      )
      .then(async function (response) {
        console.log("Load Main Page");
        var data = response.data;
        // console.log(data);

        var topicArr = Object.keys(data);
        vm.topics = topicArr;
        vm.topicData = data;

        var topicArrNum = vm.getNums();
        console.log("num:" + topicArrNum);
        console.log(topicArr.length);

        var topics = [];

        for (let i = 0; i < topicArrNum.length; i++) {
          topics.push(topicArr[topicArrNum[i]]);
        }
        //console.log(topics[0]);
        vm.topics = topics;

        var count = [];
        var i;
        for (i in data) {
          for (let j = 0; j < topics.length; j++) {
            if (i === topics[j]) {
              //console.log(topics[j])
              count.push(data[i]);
            }
          }
        }

        console.log("count:" + count);
        //console.log(json.toy)

        var dataset = {
          children: [
            { Name: topics[0], Count: count[0] },
            { Name: topics[1], Count: count[1] },
            { Name: topics[2], Count: count[2] },
            { Name: topics[3], Count: count[3] },
            { Name: topics[4], Count: count[4] },
            { Name: topics[5], Count: count[5] },
            { Name: topics[6], Count: count[6] },
            { Name: topics[7], Count: count[7] },
            { Name: topics[8], Count: count[8] },
            { Name: topics[9], Count: count[9] },
            { Name: topics[10], Count: count[10] },
            { Name: topics[11], Count: count[11] },
            { Name: topics[12], Count: count[12] },
            { Name: topics[13], Count: count[13] },
            { Name: topics[14], Count: count[14] },
            { Name: topics[15], Count: count[15] },
            { Name: topics[16], Count: count[16] },
            { Name: topics[17], Count: count[17] },
            { Name: topics[18], Count: count[18] },
            { Name: topics[19], Count: count[19] },
          ],
        };
        vm.generateBubble(dataset);
      });
  },
};
</script>
