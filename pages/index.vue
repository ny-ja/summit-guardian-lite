<script setup>
import { ref, onMounted } from "vue";
import { getDataFromDatabase, onDatabaseDataChange } from "../firebase";

useHead({
    title: "Welcome - Summit Guardian",
});

const databaseData = ref({});
const isHeartRateAbnormal = ref(false);

onMounted(async () => {
    try {
        const paths = [
            "sensor/heartRate",
        ];

        for (const path of paths) {
            const data = await getDataFromDatabase(path);
            databaseData.value[path] = data;

            // Listen for changes in each path
            onDatabaseDataChange(path, (newData) => {
                databaseData.value[path] = newData || "Initializing...";

                if (path === "sensor/heartRate") {
                    isHeartRateAbnormal.value = newData < 60 || newData > 100;
                }
            });
        }
    } catch (error) {
        console.error("Error fetching data from Firebase:", error);
    }
});
</script>

<template>
    <div class="antialiased bg-white dark:bg-black dark:text-white flex flex-col items-center justify-center min-h-screen place-content-center sm:text-base text-black text-sm"
        data-v-8e92eaa8="">
        <div class="flex flex-1 flex-col gap-y-16 py-14" data-v-8e92eaa8="">
            <div class="flex flex-col gap-y-4 items-center justify-center" data-v-8e92eaa8="">
                <div data-v-8e92eaa8="">
                    <img class="size-24" src="/svgs/logos/logo-default.svg" alt="logo" />
                </div>
                <h1 class="dark:text-white font-semibold sm:text-5xl text-4xl text-black text-center"
                    data-v-8e92eaa8="">
                    Welcome to Summit Guardian!
                </h1>
                <p class="px-4">
                    A Comprehensive IOT soluttion for mountain climber health monitoring
                    and GPS tracking in search and rescue operations.
                </p>
            </div>
            <div class="gap-6 grid grid-cols-2 lg:grid-cols-10 max-w-[960px] px-4" data-v-8e92eaa8="">
                <DistressSignal />
                <div class="border border-gray-200 col-span-2 dark:border-transparent dark:text-white hover:border-transparent items-center justify-center lg:col-span-6 lg:min-h-min md:min-h-[180px] thermometers-container relative rounded-xl sm:col-span-1 sm:min-h-[220px] text-black"
                    data-v-8e92eaa8="">
                    <div class="gradient-border gradient-border-thermometers gradient-border-rect" data-v-8e92eaa8="">
                    </div>
                    <div class="absolute bg-gradient-to-l duration-300 from-yellow-400 inset-y-0 modules-gradient-right right-0 rounded-xl to-transparent transition-opacity w-[20%] z-1"
                        data-v-8e92eaa8=""></div>
                    <a class="bg-white dark:bg-gray-900 dark:border-none flex gap-x-4 items-center justify-center lg:min-h-min md:min-h-[180px] px-5 py-6 rounded-xl sm:min-h-[220px]"
                        data-v-8e92eaa8="">
                        <img src="/svgs/thermometers/thermometer-light-hovered.svg" alt="thermometer icon"
                            class="thermometer-light-hovered size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/thermometers/thermometer-dark-hovered.svg" alt="thermometer icon"
                            class="thermometer-dark-hovered size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/thermometers/thermometer-light-default.svg" alt="thermometer icon"
                            class="thermometer-light-default size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/thermometers/thermometer-dark-default.svg" alt="thermometer icon"
                            class="thermometer-dark-default size-18" data-v-8e92eaa8="" />
                        <div class="dark:text-white flex flex-col space-y text-black" data-v-8e92eaa8="">
                            <h3 class="font-semibold text-xl" data-v-8e92eaa8="">
                                Body Temperature
                            </h3>
                            <p class="dark:text-gray-300 text-gray-700" data-v-8e92eaa8="">
                                Track and monitor your body temperature to detect early signs of
                                illness.
                            </p>
                            <span
                                class="text-xl dark:text-gray-300 text-gray-700 bg-gray-100 dark:bg-white/10 font-bold font-mono p-1 rounded"
                                data-v-8e92eaa8="">36.4 °C</span>
                        </div>
                        <div
                            class="absolute inset-0 flex items-center justify-center text-xl font-bold rounded-xl bg-white/80 text-black dark:bg-black/60 dark:text-white">
                            Coming Soon
                        </div>
                    </a>
                </div>
                <div class="border border-gray-200 col-span-2 dark:border-transparent dark:text-white heartbeats-container hover:border-transparent items-center justify-center lg:col-span-4 lg:order-none order-last relative rounded-xl row-span-2 text-black"
                    data-v-8e92eaa8="">
                    <div class="gradient-border gradient-border-heartbeats gradient-border-square" data-v-8e92eaa8="">
                    </div>
                    <a class="bg-white dark:bg-gray-900 flex gap-y-4 items-center justify-center lg:flex-col rounded-xl"
                        data-v-8e92eaa8="">
                        <div class="flex flex-col gap-y-2 items-center justify-center lg:flex-col lg:py-7 px-5 py-6 rounded-xl sm:flex-row"
                            data-v-8e92eaa8="">
                            <div class="dark:text-white flex flex-col space-y text-black" data-v-8e92eaa8="">
                                <h3 class="font-semibold text-xl" data-v-8e92eaa8="">
                                    Heart Rate
                                </h3>
                                <p class="dark:text-gray-300 text-gray-700" data-v-8e92eaa8="">
                                    Monitor and track your heart rate in real time for better
                                    health insights.
                                </p>
                                <span
                                    :class="['text-xl dark:text-gray-300 text-gray-700 bg-gray-100 dark:bg-white/10 font-bold font-mono p-1 rounded', { 'dark:text-red-300 text-red-700 shadow-2xl shadow-red-500 border border-red-500': isHeartRateAbnormal }]">{{
                                        databaseData["sensor/heartRate"] || "Initializing..."
                                    }}
                                    bpm</span>
                            </div>
                            <img src="/svgs/heartbeats/heartbeat-with-background-light-hovered.svg" alt="heartbeat icon"
                                class="heartbeat-with-background-light-hovered h-32 sm:h-34" data-v-8e92eaa8="" />
                            <img src="/svgs/heartbeats/heartbeat-with-background-dark-hovered.svg" alt="heartbeat icon"
                                class="heartbeat-with-background-dark-hovered h-32 sm:h-34" data-v-8e92eaa8="" />
                            <img src="/svgs/heartbeats/heartbeat-with-background-light-default.svg" alt="heartbeat icon"
                                class="heartbeat-with-background-light-default h-32 sm:h-34" data-v-8e92eaa8="" />
                            <img src="/svgs/heartbeats/heartbeat-with-background-dark-default.svg" alt="heartbeat icon"
                                class="heartbeat-with-background-dark-default h-32 sm:h-34" data-v-8e92eaa8="" />
                        </div>
                    </a>
                </div>
                <div class="border border-gray-200 col-span-2 dark:border-transparent dark:text-white bloods-container hover:border-transparent items-center justify-center lg:col-span-6 lg:min-h-min md:min-h-[180px] relative rounded-xl sm:col-span-1 sm:min-h-[220px] text-black"
                    data-v-8e92eaa8="">
                    <div class="gradient-border gradient-border-bloods gradient-border-rect" data-v-8e92eaa8=""></div>
                    <div class="absolute bg-gradient-to-l duration-300 bloods-gradient-right from-red-400 inset-y-0 right-0 rounded-xl to-transparent transition-opacity w-[20%] z-1"
                        data-v-8e92eaa8=""></div>
                    <a class="bg-white dark:bg-gray-900 flex gap-x-4 items-center justify-center lg:min-h-min md:min-h-[180px] px-5 py-6 rounded-xl sm:min-h-[220px]"
                        data-v-8e92eaa8="">
                        <img src="/svgs/bloods/blood-light-hovered.svg" alt="blood icon"
                            class="blood-light-hovered size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/bloods/blood-dark-hovered.svg" alt="blood icon"
                            class="blood-dark-hovered size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/bloods/blood-light-default.svg" alt="blood icon"
                            class="blood-light-default size-18" data-v-8e92eaa8="" />
                        <img src="/svgs/bloods/blood-dark-default.svg" alt="blood icon"
                            class="blood-dark-default size-18" data-v-8e92eaa8="" />
                        <div class="dark:text-white flex flex-col space-y text-black" data-v-8e92eaa8="">
                            <h3 class="font-semibold text-xl" data-v-8e92eaa8="">
                                Blood Oxygen
                            </h3>
                            <p class="dark:text-gray-300 text-gray-700" data-v-8e92eaa8="">
                                Measure your blood oxygen saturation levels to monitor
                                respiratory health.
                            </p>
                            <span
                                class="text-xl dark:text-gray-300 text-gray-700 bg-gray-100 dark:bg-white/10 font-bold font-mono p-1 rounded"
                                data-v-8e92eaa8="">99 %</span>
                        </div>
                        <div
                            class="absolute inset-0 flex items-center justify-center text-xl font-bold rounded-xl bg-white/80 text-black dark:bg-black/60 dark:text-white">
                            Coming Soon
                        </div>
                    </a>
                </div>
            </div>
        </div>
        <footer
            class="bg-white border-gray-200 border-t dark:bg-black dark:border-gray-900 flex h-[70px] items-center relative w-full"
            data-v-8e92eaa8="">
            <div class="-top-4 absolute flex inset-x-0 items-center justify-center" data-v-8e92eaa8="">
                <div data-v-8e92eaa8="">
                    <img class="size-8 bg-white dark:bg-[#18181B] rounded-full p-2" src="/svgs/logos/logo-default.svg"
                        alt="logo" />
                </div>
            </div>
            <div class="lg:px-8 mx-auto px-4 sm:px-6 w-full" data-v-8e92eaa8="">
                <div class="flex flex-col gap-3 items-center sm:flex-row sm:justify-between" data-v-8e92eaa8="">
                    <div class="flex flex-col-reverse gap-3 items-center sm:flex-row" data-v-8e92eaa8="">
                        <span class="dark:text-gray-300 text-gray-700 text-sm" data-v-8e92eaa8="">© 2025 Summit
                            Guardian</span>
                    </div>
                    <ul class="flex gap-3 items-center justify-end" data-v-8e92eaa8="">
                        <li data-v-8e92eaa8="">
                            <a href="https://github.com/ny-ja/summit-guardian-lite" target="_blank"
                                class="dark:hover:text-white dark:text-gray-300 focus-visible:ring-2 hover:text-black text-gray-700"
                                data-v-8e92eaa8=""><span class="sr-only" data-v-8e92eaa8="">Summit Guardian GitHub
                                    Repository</span>
                                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" fill="none"
                                    data-v-8e92eaa8="">
                                    <path fill="currentColor"
                                        d="M9.153.793a8.334 8.334 0 0 0-2.636 16.24c.417.072.573-.178.573-.396 0-.198-.01-.854-.01-1.552-2.094.385-2.636-.51-2.802-.98a3 3 0 0 0-.854-1.177c-.292-.156-.709-.541-.01-.552a1.67 1.67 0 0 1 1.28.854 1.78 1.78 0 0 0 2.427.688c.036-.424.225-.82.532-1.115-1.854-.208-3.792-.927-3.792-4.114a3.24 3.24 0 0 1 .854-2.24A3 3 0 0 1 4.8 4.241s.697-.219 2.291.854a7.86 7.86 0 0 1 4.167 0c1.594-1.083 2.292-.854 2.292-.854.308.698.338 1.488.083 2.208.562.61.868 1.411.854 2.24 0 3.198-1.948 3.906-3.802 4.114a1.97 1.97 0 0 1 .562 1.542c0 1.115-.01 2.01-.01 2.292 0 .218.156.479.573.396A8.338 8.338 0 0 0 9.153.793"
                                        data-v-8e92eaa8=""></path>
                                </svg>
                            </a>
                        </li>
                    </ul>
                </div>
            </div>
        </footer>
    </div>
</template>
