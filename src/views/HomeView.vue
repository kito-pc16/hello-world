<script setup lang="ts">
import ZiptoAddress from '@/components/ZiptoAddress.vue';
import WeatherOverview from '@/components/WeatherOverview.vue';
</script>

<template>
    <div class="border border-sky-400 rounded-md p-2 m-2 w-fit">
        <ZiptoAddress />
    </div>
    <div class="border border-sky-400 rounded-md p-2 m-2 w-fit">
        <WeatherOverview />
    </div>
</template>