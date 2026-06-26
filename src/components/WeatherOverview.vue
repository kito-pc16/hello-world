<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import { Temporal } from 'temporal-polyfill'

interface WeatherData {
  publishingOffice: string
  reportDatetime: string
  targetArea: string
  headlineText: string
  text: string
}

const areacodes = ref([
  { code: '011000', name: '宗谷地方' },
  { code: '012000', name: '上川・留萌地方' },
  { code: '013000', name: '網走・北見・紋別地方' },
  { code: '014100', name: '釧路・根室地方' },
  { code: '015000', name: '胆振・日高地方' },
  { code: '016000', name: '石狩・空知・後志地方' },
  { code: '017000', name: '渡島・檜山地方' },
  { code: '020000', name: '青森県' },
  { code: '030000', name: '岩手県' },
  { code: '040000', name: '宮城県' },
  { code: '050000', name: '秋田県' },
  { code: '060000', name: '山形県' },
  { code: '070000', name: '福島県' },
  { code: '080000', name: '茨城県' },
  { code: '090000', name: '栃木県' },
  { code: '100000', name: '群馬県' },
  { code: '110000', name: '埼玉県' },
  { code: '120000', name: '千葉県' },
  { code: '130000', name: '東京都' },
  { code: '140000', name: '神奈川県' },
  { code: '150000', name: '新潟県' },
  { code: '160000', name: '富山県' },
  { code: '170000', name: '石川県' },
  { code: '180000', name: '福井県' },
  { code: '190000', name: '山梨県' },
  { code: '200000', name: '長野県' },
  { code: '210000', name: '岐阜県' },
  { code: '220000', name: '静岡県' },
  { code: '230000', name: '愛知県' },
  { code: '240000', name: '三重県' },
  { code: '250000', name: '滋賀県' },
  { code: '260000', name: '京都府' },
  { code: '270000', name: '大阪府' },
  { code: '280000', name: '兵庫県' },
  { code: '290000', name: '奈良県' },
  { code: '300000', name: '和歌山県' },
  { code: '310000', name: '鳥取県' },
  { code: '320000', name: '島根県' },
  { code: '330000', name: '岡山県' },
  { code: '340000', name: '広島県' },
  { code: '350000', name: '山口県' },
  { code: '360000', name: '徳島県' },
  { code: '370000', name: '香川県' },
  { code: '380000', name: '愛媛県' },
  { code: '390000', name: '高知県' },
  { code: '400000', name: '福岡県' },
  { code: '410000', name: '佐賀県' },
  { code: '420000', name: '長崎県' },
  { code: '430000', name: '熊本県' },
  { code: '440000', name: '大分県' },
  { code: '450000', name: '宮崎県' },
  { code: '460000', name: '鹿児島県' },
  { code: '460040', name: '奄美地方' },
  { code: '471000', name: '沖縄本島地方' },
  { code: '472000', name: '大東島地方' },
  { code: '473000', name: '宮古島地方' },
  { code: '474000', name: '八重山地方' }
])
const weatherData = ref<WeatherData | null>(null)
const yohoukucode = ref('')
const reportDatetime = ref('')

// Fetch weather data from the JMA API
async function fetchWeatherData(yohoukucode: string) {
  await axios.get<WeatherData>('https://www.jma.go.jp/bosai/forecast/data/overview_forecast/'+yohoukucode+'.json')
    .then(response => {
      weatherData.value = response.data as WeatherData
      reportDatetime.value = Temporal.Instant.from(response.data.reportDatetime).toLocaleString()
    })
    .catch(error => {
      console.error('Error fetching weather data:', error)
    })
}
</script>

<template>
  <div class="flex flex-col items-center justify-center gap-4">
    <h1 class="text-2xl font-bold">天気予報の概要</h1>
    <div class="flex items-center gap-2">
      <p>予報区</p>
      <select v-model="yohoukucode" class="border p-2 rounded">
        <option v-for="area in areacodes" :key="area.code" :value="area.code">{{ area.name }}</option>
      </select>
      <button class="bg-blue-500 text-white p-2 rounded" @click="fetchWeatherData(yohoukucode)">
        <div class="flex items-center">
        <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth={1.5} stroke="currentColor" className="size-6">
          <path strokeLinecap="round" strokeLinejoin="round" d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z" />
        </svg>
        検索
        </div>
      </button>
    </div>
    <div class="flex gap-4 text-xl">
      <p>{{ reportDatetime }}</p>
      <p>{{ weatherData?.publishingOffice }} 発表</p>
    </div>
    <div v-if="weatherData" class="mt-2 p-4 border rounded bg-gray-100 w-full max-w-md">
      <h2 class="text-xl font-semibold mb-2">{{weatherData.targetArea}}の天気概要</h2>
      <p>{{ weatherData.text }}</p>
    </div>
  </div>
</template>