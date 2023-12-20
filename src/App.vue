<script setup>
import {ref, reactive, onMounted, watch} from "vue";
import axios from 'axios'

const url = 'https://webapi.sporttery.cn/gateway/jc/football/getMatchResultV1.qry?matchPage=1&leagueId=&pageNo=1&isFix=0&pcOrWap=1'
const dataL = reactive([])

onMounted(() => {
  fetchData()
})

const fetchData = async () => {
  axios.get(url+'&matchBeginDate=2023-10-01&matchEndDate=2023-12-20&pageSize=30000').then(res => {
    dataL.value = res.data.value.matchResult
  })
}

const filteredData = ref([])


const getbettingSingleStyle = (item) => {
  if (item.bettingSingle === 1) {
    return {
      // border: "1px solid red",
      color: "red"
    };
  }
  return {};
}

</script>

<template>
  <div id="app">
    <table class="table">
      <thead>
      <tr>
        <th>赛事日期</th>
        <th>赛事编号</th>
        <th>联赛</th>
        <th>主队（让球）vs客队</th>
        <th>半场比分</th>
        <th>全场比分</th>
        <th>胜</th>
        <th>平</th>
        <th>负</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="item in dataL.value" :key="item.matchId">
        <template v-if="item.matchNumStr.includes('001')">
          <td>{{ item.matchDate }}</td>
          <td>{{ item.matchNumStr }}</td>
          <td>{{ item.leagueName }}</td>
          <td :style="getbettingSingleStyle(item)">{{ item.homeTeam }}({{ item.goalLine }})VS{{ item.awayTeam }}</td>
          <td>{{ item.sectionsNo1 }}</td>
          <td>{{ item.sectionsNo999 }}</td>
          <td>{{ item.h }}</td>
          <td>{{ item.d }}</td>
          <td>{{ item.a }}</td>
        </template>
      </tr>
      </tbody>
    </table>
  </div>
</template>


<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  padding: 8px;
  border: 1px solid #ccc;
}

.table th {
  background-color: #f0f0f0;
  font-weight: bold;
}
</style>
