<template>
  <div>
    <v-row class="mx-5 my-5">
      <v-text-field
        hide-details
        single-line
        v-model="searchedVideo"
        @keyup="checkForEnterKey"
      ></v-text-field>
      <v-btn text @click="getVideo()"><v-icon>mdi-magnify</v-icon></v-btn>
    </v-row>
    <v-row class="align-center">
      <videoCard
        class="mx-2"
        v-for="(v, key) in selectedVideos"
        :key="key"
        :video="v"
      />
    </v-row>
  </div>
</template>

<script>
import videoCard from "../components/video.vue";
import axios from "axios";
export default {
  components: {
    videoCard,
  },
  data() {
    return {
      allVideos: [],
      searchedVideo: "",
      selectedVideos: [],
      videos: [],
      videoTitle: [],
    };
  },
  methods: {
    getVideo() {
      if (this.videoTitle.includes(this.searchedVideo)) {
        var ind = this.videoTitle.indexOf(this.searchedVideo);
        this.selectedVideos = [this.selectedVideos[ind]];
        console.log(this.selectedVideos);
      } else {
        alert("Not Found");
      }
    },
    checkForEnterKey(event) {
      if (event.keyCode == 13) {
        this.getVideo();
      }
    },
  },
  mounted() {
    this.$store.state.curpage = "Find Your Video";
    var vm = this;
    var searchedTopic = this.$route.params.topicname;
    axios
      .get(
        "https://raw.githubusercontent.com/YufeiLinUlysses/CS573_Final_Project/zeng/pages/data/ted_talk.json"
      )
      .then(async function (response) {
        var data = response.data;
        console.log(data);
        vm.allVideos = data;
        var qualified = [];
        var qualifiedTitle = [];
        for (var d in data) {
          var cur = data[d];
          if (cur["tags"].includes(searchedTopic)) {
            qualified.push(cur);
            qualifiedTitle.push(cur["title"]);
          }
        }
        vm.selectedVideos = qualified;
        vm.videoTitle = qualifiedTitle;
      });
  },
};
</script>
