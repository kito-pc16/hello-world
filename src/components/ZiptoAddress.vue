<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

interface address {
    address1: string
    address2: string
    address3: string
    kana1: string
    kana2: string
    kana3: string
    prefcode: number
    zipcode: string
}
interface res {
    message: string,
    results: address[]
    status: number
}

const zipcode = ref();
const result = ref<res>();

// 郵便番号（数字7桁、ハイフンなし）から住所データを取得する関数
async function getAddress(zipcode: string){
    const params = new URLSearchParams();
    params.append('zipcode', zipcode);

    await axios.post('https://zipcloud.ibsnet.co.jp/api/search', params)
    .then((response) => {
        result.value = response.data as res;
    })
    .catch((error) => {
        console.error("Error Message:", error);
    })
}

// 変数をクリアして フォーカスをinput項目に移す
function clearData() {
    zipcode.value = '';
    result.value!.message = '';
    result.value!.results = [];
    result.value!.status = 0;

    const item = document.getElementById('zipcode') as HTMLInputElement;
    item.focus();
}
</script>

<template>
    <p class="text-xl font-bold">郵便番号 ⇒ 住所 検索</p>

    <div class="flex items-center gap-3 my-3">
        <p>郵便番号(ハイフンなし、数字7桁):</p>
        <input id="zipcode" class="border border-gray-300 rounded-md p-2" type="text" v-model="zipcode"/>
        <button class="bg-sky-400 hover:bg-sky-600 rounded-md p-2 text-white" type="button" @click="getAddress(zipcode)">
            <div class="flex items-center">
                <svg class="h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth={1.5} stroke="currentColor" className="size-6">
                    <path strokeLinecap="round" strokeLinejoin="round" d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z" />
                </svg>
                検索
            </div>
        </button>
        <button class="bg-gray-100 hover:bg-gray-300 rounded-md p-2" type="button" @click="clearData()">クリア</button>
    </div>
    <p v-if="result != undefined && result.status == 400">{{ result!.message }}</p>
    <p v-if="result != undefined && result.status == 200 && result.results == null">該当するデータがありません</p>

    <div>
        <ul v-if="result != undefined && result.results != null">
            <li>郵便番号 : {{ result?.results[0]?.zipcode }}</li>
            <li>都道府県コード : {{ result?.results[0]?.prefcode }}</li>
            <li>都道府県名 : {{ result?.results[0]?.address1 }}</li>
            <li>市区町村名 : {{ result?.results[0]?.address2 }}</li>
            <li>町域名 : {{ result?.results[0]?.address3 }}</li>
            <li>都道府県カナ : {{ result?.results[0]?.kana1 }}</li>
            <li>市区町村カナ : {{ result?.results[0]?.kana2 }}</li>
            <li>町域名カナ : {{ result?.results[0]?.kana3 }}</li>
        </ul>
    </div>
</template>