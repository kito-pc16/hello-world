<script setup lang="ts">
import { CakeIcon } from '@heroicons/vue/16/solid';
</script>
<template>
    <div class="flex flex-col gap-4 border border-indigo-400 rounded-md p-2 m-2 shadow-md w-fit">
        <div class="flex gap-2">
            <CakeIcon class="w-6 h-6" />
            <p>This Page About View.</p>
        </div>
        <div class="flex items-end gap-4">
            <p class="text-xl font-bold">郵便番号 ⇒ 住所 検索</p>
            <p>ver. 0.10</p>
        </div>
        <p class="flex justify-end">2026.6.25.</p>
    </div>
</template>