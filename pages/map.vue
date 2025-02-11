<script setup>
useHead({
    title: 'Map - Summit Guardian',
});

const router = useRouter();

const goBack = () => {
    router.back();
};
</script>

<template>
    <div class="min-h-screen flex items-center justify-center bg-white dark:bg-black dark:text-white">
        <div class="relative w-full max-w-4xl p-6 sm:p-8 lg:p-10 bg-gray-100 dark:bg-zinc-900 shadow-lg rounded-lg">
            
            <button 
                @click="goBack" 
                type="button"
                aria-label="Go back"
                class="absolute top-4 left-4 z-10 flex h-10 w-10 items-center justify-center rounded-full bg-white shadow-md ring-1 ring-zinc-900/5 transition hover:bg-gray-200 dark:border dark:border-zinc-700 dark:bg-zinc-800 dark:hover:bg-zinc-700 cursor-pointer"
            >
                <svg viewBox="0 0 16 16" fill="none" aria-hidden="true"
                    class="h-4 w-4 transition stroke-zinc-500 group-hover:stroke-zinc-700 dark:group-hover:stroke-zinc-400">
                    <path d="M7.25 11.25 3.75 8m0 0 3.5-3.25M3.75 8h8.5" stroke-width="1.5"
                        stroke-linecap="round" stroke-linejoin="round"></path>
                </svg>
            </button>

            <div class="mt-12">
                <Map />
            </div>
        </div>
    </div>
</template>
