<template>
  <button @click="getResponse">요청 보내기</button>
  <article id="response-area">

  </article>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      ex : 0,

    }
  },
  methods : {
    async getResponse() {
      const area = document.getElementById("response-area");

      try {
        const response = await fetch("https://dapi.kakao.com/v2/local/search/address.json? " + new URLSearchParams({
          analyze_type: "similar",
          page: 1,
          size: 10,
          query: "경기도 구리시"
        }).toString(), {
          headers : {
            Authorization : "KakaoAK " + process.env.VUE_APP_REST_API_KEY
          }
        });

        if (!response.ok) {
          throw new Error("데이터를 가져오는 도중 오류 발생");
        }

        const data = await response.json();

        area.textContent = JSON.stringify(data);

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
</style>
