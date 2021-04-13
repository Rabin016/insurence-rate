<template>
    <div>
        <div class="mx-auto">
            <div class="bg-white shadow-xl rounded-lg overflow-hidden p-5">
                <form @submit.prevent="calculate">
                    <div>
                        <label for="limit">Limit amount: </label>
                        <input
                            type="number"
                            name="limit"
                            v-model="limit"
                            class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100 w-full"
                        />
                    </div>

                    <div class="py-3">
                        <input
                            class="text-red-500 rounded cursor-pointer"
                            type="checkbox"
                            v-model="tenpercent"
                        />
                        <label for="tenPercent"> Add +10%</label>
                    </div>
                    <div class="border-2 rounded-lg px-1 py-3">
                        <div class="flex align-middle items-center py-2">
                            <p class="pr-3">
                                Add Occupation of premises:
                            </p>
                            <button
                                class="bg-green-300 p-1 rounded-lg text-gray-800 shadow hover:bg-red-300 transition-colors duration-300 hover:shadow-lg"
                                @click="addPremises"
                            >
                                <svg
                                    class="w-6 h-6"
                                    fill="none"
                                    stroke="currentColor"
                                    viewBox="0 0 24 24"
                                    xmlns="http://www.w3.org/2000/svg"
                                >
                                    <path
                                        stroke-linecap="round"
                                        stroke-linejoin="round"
                                        stroke-width="2"
                                        d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
                                    ></path>
                                </svg>
                            </button>
                        </div>
                        <div class="space-y-4">
                            <div
                                v-for="structure in structures"
                                :key="structure._id"
                                class="p-1 "
                            >
                                <div
                                    class="bg-gray-100 rounded-xl p-1 shadow-md space-y-2 py-4 lg:px-3 px-1"
                                >
                                    <div>
                                        <label>Constructure Name </label>
                                        <select
                                            class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                            v-model="structure.occPro"
                                        >
                                            <option value="">Select one</option>
                                            <option value="godown"
                                                >Godown</option
                                            >
                                            <option value="showroom"
                                                >Showroom</option
                                            >
                                        </select>
                                    </div>
                                    <div>
                                        <label>Constructure Type: </label>
                                        <select
                                            class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                            v-model="structure.conType"
                                        >
                                            <option value="">Select one</option>
                                            <option value="class1"
                                                >Class I</option
                                            >
                                            <option value="class2"
                                                >Class II</option
                                            >
                                        </select>
                                    </div>
                                    <div>
                                        <label for="itemAmount"
                                            >Item percentage in
                                            {{ structure.occPro }}</label
                                        >
                                        <div class="flex items-center">
                                            <input
                                                class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100 w-full"
                                                type="number"
                                                v-model="
                                                    structure.itemPercentage
                                                "
                                            />
                                            <span>%</span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="py-3">
                        <label>Item Type: </label>
                        <select
                            class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                            v-model="itemType"
                        >
                            <option value="">Select one</option>
                            <option value="non">Non-Hazerd</option>
                        </select>
                    </div>
                    <div class="pb-3">
                        <input
                            class="text-red-500 rounded"
                            type="checkbox"
                            v-model="rsd"
                        />
                        <label for="rsd"> RSD</label>
                    </div>
                    <button
                        class="bg-green-400 px-3 py-2 rounded-lg hover:bg-red-400 transition-colors duration-300 ring-4 ring-green-200"
                        type="submit"
                    >
                        Calculate
                    </button>
                </form>
            </div>
            <div class="py-3 bg-gray-100 mt-5 rounded-lg px-2">
                <p>Premium detail:</p>
                <p>
                    Net premium:
                    <span class="text-red-500">{{
                        netPremium | roundFig | numberWithCommas
                    }}</span>
                </p>
                <p>
                    Vat:
                    <span class="text-red-500">{{
                        vat | roundFig | numberWithCommas
                    }}</span>
                </p>

                <p>
                    Total:
                    <span class="text-red-500">{{
                        total | roundFig | numberWithCommas
                    }}</span>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
export default {
    name: "HelloWorld",
    data() {
        return {
            limit: null,
            tenpercent: true,
            netPremium: 0,
            vat: null,
            total: null,
            rsd: false,
            structures: [
                {
                    _id: uuidv4(),
                    occPro: "godown",
                    conType: "class1",
                    itemPercentage: 100,
                },
            ],
            itemType: "non",
        };
    },
    filters: {
        roundFig(val) {
            if (!val) return "";
            return Math.round(val);
        },
        numberWithCommas(val) {
            return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        },
    },
    methods: {
        addPremises() {
            this.structures.push({
                _id: uuidv4(),
                occPro: "godown",
                conType: "class1",
                itemPercentage: null,
            });
        },
        calculate() {
            this.netPremium = 0;
            this.vat = 0;
            this.total = 0;
            let amount;
            this.limit = parseInt(this.limit);
            this.netPremium = parseInt(this.netPremium);
            this.vat = parseInt(this.vat);
            this.total = parseInt(this.total);
            if (this.tenpercent) {
                amount = this.limit * (10 / 100);
                amount = parseInt(amount) + this.limit;
            } else {
                amount = this.limit;
            }
            // Structure
            this.structures.forEach((structure) => {
                let structurePrice = amount * (structure.itemPercentage / 100);
                parseInt(structurePrice);
                console.log(structurePrice);

                if (structure.occPro == "godown") {
                    if (structure.conType == "class1") {
                        this.netPremium =
                            this.netPremium + structurePrice * (0.11 / 100);
                    }
                    if (structure.conType == "class2") {
                        this.netPremium =
                            this.netPremium + structurePrice * (0.13 / 100);
                    }
                }
                if (structure.occPro == "showroom") {
                    if (structure.conType == "class1") {
                        this.netPremium =
                            this.netPremium + structurePrice * (0.15 / 100);
                    }
                    if (structure.conType == "class2") {
                        this.netPremium =
                            this.netPremium + structurePrice * (0.17 / 100);
                    }
                }
            });
            // RSD
            if (this.rsd) {
                this.netPremium = this.netPremium + amount * (0.13 / 100);
            }
            // VAT
            this.vat = this.netPremium * (15 / 100);
            // Total
            this.total = this.netPremium + this.vat;
        },
    },
};
</script>

<style></style>
