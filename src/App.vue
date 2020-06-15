<template>
  <div id="app">
    <b-container>
      <b-row>
        <h1 class="mx-auto">font3d</h1>
      </b-row>
      <b-row>
        <p class="mx-auto">A web app to generate a 3D STL text model</p>
      </b-row>
      <hr />

      <b-row>
        <b-col>
          <h1>Step 1</h1>
        </b-col>
        <b-col>
          <h1>Step 2</h1>
        </b-col>
        <b-col>
          <h1>Step 3</h1>
        </b-col>
      </b-row>
      <hr />
      <b-row>
        <b-col>
          <h3>Enter some text and download the STL file.</h3>
        </b-col>
        <b-col>
          <h3>
            Check the STL file
            and Slice with your favourite G Code processing tool
          </h3>
        </b-col>
        <b-col>
          <h3>Start your print with your 3D printer.</h3>
        </b-col>
      </b-row>
      <b-row>
        <b-col>Only upper case letters, numbers and space is allowed.</b-col>
        <b-col>
          e.g.
          <a href="http://slic3r.org/" target="_blank">Slic3r</a>,
          <a href="http://wiki.ultimaker.com/Cura" target="_blank">Cura</a>
        </b-col>
        <b-col>
          If you don't have one yet, here's a few good choices.
          <br />(
          <a
            href="http://printrbot.com/shop/assembled-simple-metal/"
            target="_blank"
          >Printrbot Simple Metal</a>,
          <a href="http://cubify.com/" target="_blank">Cubify</a>,
          <a href="http://www.makerbot.com/" target="_blank">Makerbot</a>
          )
        </b-col>
      </b-row>
      <hr />
      <b-row class="mt-5 mb-5">
        <b-col sm="10">
          <b-form-input v-model="word" placeholder="Type the desired word here"></b-form-input>
        </b-col>
        <b-col sm="2">
          <b-button v-on:click="generate">Generate STL File</b-button>
        </b-col>
      </b-row>
      <hr />
      <b-row>
        <p class="mx-auto">
          Designed by
          <a href="http://cameronlai.com">Cameron Lai</a>
        </p>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data: function() {
    return {
      word: null
    };
  },
  methods: {
    generate: function() {
      axios
        .post("https://font3d-juuc7vo4na-de.a.run.app/generate", {
          word: this.word.toUpperCase(),
          responseType: "blob"
        })
        .then(function(response) {
          console.log(response);
          var fileURL = window.URL.createObjectURL(new Blob([response.data]));
          var fileLink = document.createElement("a");

          fileLink.href = fileURL;
          fileLink.setAttribute("download", "file.pdf");
          document.body.appendChild(fileLink);

          fileLink.click();
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
