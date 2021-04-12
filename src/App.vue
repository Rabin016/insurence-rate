<template>
    <div
        class="bg-gradient-to-r from-rose-500 via-red-400 to-red-500 font-pop"
        id="app"
    >
        <div class="mx-auto w-full sm:w-1/2 lg:w-2/3 pb-3 px-3 ">
            <div class="bg-gray-100 rounded-b-md py-3">
                <h1 class=" text-center font-bold text-3xl text-gray-800">
                    Insurence <span class="text-red-500">Rate</span>
                    <p class="text-center text-xs">(Beta Version)</p>
                </h1>
                <div class=" text-center">
                    <router-link
                        exact-active-class="text-red-500"
                        class="hover:text-red-700 hover:underline"
                        to="/"
                        >Fire</router-link
                    >
                    |
                    <router-link
                        exact-active-class="text-red-500"
                        class=" hover:text-red-700 hover:underline"
                        to="/about"
                        >Marin</router-link
                    >
                </div>
            </div>
        </div>
        <router-view />
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@500;600;700&display=swap");
.font-pop {
    font-family: "Poppins", sans-serif;
}
</style>
