<template>
    <div>
        <Suspense>
            <AsyncCityVIew />
            <template #fallback>
                <CityCardSkeleton />
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import AsyncCityVIew from '@/components/AsyncCityVIew.vue';
import CityCardSkeleton from '@/components/CityCardSkeleton.vue';

</script>
