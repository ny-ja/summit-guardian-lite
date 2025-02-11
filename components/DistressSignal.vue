<script setup>
import { ref, onMounted } from "vue";
import { getDataFromDatabase, onDatabaseDataChange } from "../firebase";

const databaseData = ref({});

onMounted(async () => {
    try {
        const paths = ["distressSignal"];

        for (const path of paths) {
            const data = await getDataFromDatabase(path);
            databaseData.value[path] = data;

            // Listen for changes in each path
            onDatabaseDataChange(path, (newData) => {
                databaseData.value[path] = newData;
            });
        }
    } catch (error) {
        console.error("Error fetching data from Firebase:", error);
    }
});
</script>

<template>
    <div v-if="databaseData.distressSignal" class="col-span-2 distress-signal-gradient-border lg:col-span-10 relative"
        data-v-8e92eaa8="">
        <div class="absolute bg-gradient-to-r duration-300 from-yellow-400 distress-signal-gradient-left inset-y-0 left-0 rounded-xl to-transparent transition-opacity w-[20%] z-1"
            data-v-8e92eaa8=""></div>
        <div class="absolute bg-gradient-to-l duration-300 from-orange-400 distress-signal-gradient-right inset-y-0 right-0 rounded-xl to-transparent transition-opacity w-[20%] z-1"
            data-v-8e92eaa8=""></div>
        <div class="-top-[58px] absolute flex inset-x-0 justify-center w-full" data-v-8e92eaa8="">
            <nuxt-link to="/map">
                <img alt="location" src="/svgs/gps/gps-dark.svg" loading="lazy" fetchpriority="high" decoding="async"
                    data-nimg="1" class="dark:block hidden" data-v-8e92eaa8="" />
                <img alt="location" src="/svgs/gps/gps-light.svg" loading="lazy" fetchpriority="high" decoding="async"
                    data-nimg="1" class="dark:hidden" data-v-8e92eaa8="" />
            </nuxt-link>
        </div>
        <div class="flex flex-col gap-y-4 items-center pb-6 pt-[58px] px-4 rounded-xl sm:px-28 z-10" data-v-8e92eaa8="">
            <h2 class="dark:text-white font-semibold text-2xl text-black" data-v-8e92eaa8="">
                Climber is in danger!
            </h2>
            <p class="mb-2 text-center" data-v-8e92eaa8="">
                <a class="bg-gray-100 dark:bg-white/10 font-bold font-mono p-1 rounded" data-v-8e92eaa8="">Immediately
                    initiate the emergency response protocol:</a>
                alert the nearest
                <span class="bg-gray-100 dark:bg-white/10 font-bold font-mono p-1 rounded" data-v-8e92eaa8="">
                    search and rescue team
                </span>
                about the distress signal and provide them with the exact coordinates of
                the mountain climber.
            </p>
        </div>
    </div>
</template>
