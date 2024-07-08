<template>
  <h1>주소 좌표 출력</h1>
  <input id="input-area" placeholder="주소 검색"/>
  <button @click="saveInput(), getResponse()">Fetch API 검색</button>
  <article id="response-area"></article>
  <button @click="saveInput(), getResponseAxios()">Axios 검색</button>
  <article id="axios-response-area"></article>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',
  data() {
    return {
      input: "",
    }
  },
  methods : {
    saveInput() {
      const area = document.getElementById("input-area");

      this.input = area.value;
      console.log("input : " + this.input);
    },

    async getResponse() {
      console.log("getResponse start");
      const area = document.getElementById("response-area");

      try {
        const response = await fetch("https://dapi.kakao.com/v2/local/search/address.json? " + new URLSearchParams({
          analyze_type: "similar",
          page: 1,
          size: 10,
          query: this.input
        }).toString(), {
          headers : {
            Authorization : "KakaoAK " + process.env.VUE_APP_REST_API_KEY
          }
        });

        if (!response.ok) {
          throw new Error("데이터를 가져오는 도중 오류 발생");
        }

        const data = await response.json();

        const div1 = document.createElement("div");
        div1.textContent = "x : " + data.documents[0].x;

        const div2 = document.createElement("div");
        div2.textContent = "y : " + data.documents[0].y;

        area.textContent = "";
        area.append(div1, div2);

      } catch (error) {
        console.error("데이터를 가져오는 도중 오류 발생", error);
      }

    },

    async getResponseAxios() {
      console.log("getResponse start");
      const area = document.getElementById("axios-response-area");

      try {
        const response = await axios.get("https://dapi.kakao.com/v2/local/search/address.json?",
            {
              params : {analyze_type: "similar", page: 1, size: 10, query: this.input},
              headers : {Authorization : "KakaoAK " + process.env.VUE_APP_REST_API_KEY},
            });

        if (response.status !== 200) {
          throw new Error("데이터를 가져오는 도중 오류 발생");
        }

        const data = await response.data;

        const div1 = document.createElement("div");
        div1.textContent = "x : " + data.documents[0].x;

        const div2 = document.createElement("div");
        div2.textContent = "y : " + data.documents[0].y;

        area.textContent = "";
        area.append(div1, div2);

      } catch (error) {
        console.error("데이터를 가져오는 도중 오류 발생", error);
      }

    }
  },
  components: {
  }
}
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
article {
  margin : 30px;
}
</style>
