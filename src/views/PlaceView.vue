<template>
    <div>
        <Suspense>
           <AsyncPlaceView/>
           
            <template #fallback>
                <p>Loading...</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>

import AsyncPlaceView from "../components/AsyncPlaceView.vue";

</script>


