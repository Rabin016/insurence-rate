<template>
    <div
        class="bg-gradient-to-r from-rose-500 via-red-400 to-red-500 font-pop pb-5"
        id="app"
    >
        <div class="mx-auto w-full sm:w-1/2 lg:w-2/3 pb-3 px-3 ">
            <div class="bg-gray-100 rounded-b-md py-3">
                <h1 class=" text-center font-bold text-3xl text-gray-800">
                    Insurence <span class="text-red-500">Rate</span>
                    <p class="text-center text-xs">(Version: 0.8 beta)</p>
                </h1>
                <div class=" text-center">
                    <router-link
                        exact-active-class="text-red-500 font-bold"
                        class="hover:text-red-700 hover:underline"
                        to="/"
                        >Fire</router-link
                    >
                    |
                    <router-link
                        exact-active-class="text-red-500 font-bold"
                        class=" hover:text-red-700 hover:underline"
                        to="/marin"
                        >Marin</router-link
                    >
                </div>
            </div>
        </div>
        <div class="flex items-center justify-center min-h-screen">
            <div class="w-full sm:w-1/2 lg:w-2/3 pb-6 px-3">
                <router-view />
            </div>
        </div>
        <div class="mx-auto w-full sm:w-1/2 lg:w-2/3">
            <div
                class="ml-auto w-1/2 sm:w-2/12 rounded-lg p-2 whitespace-no-wrap bg-gray-200"
            >
                <p class="text-gray-600 text-sm md:text-sm">Developed By:</p>
                <a href="https://life-rc.com"
                    ><img class="w-full" src="@/assets/My Logo.png" alt="logo"
                /></a>
            </div>
        </div>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@500;600;700&display=swap");
.font-pop {
    font-family: "Poppins", sans-serif;
}
</style>
