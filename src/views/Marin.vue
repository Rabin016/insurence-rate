<template>
    <div class="mx-auto">
        <div class="bg-white shadow-xl rounded-lg overflow-hidden p-5">
            <form @submit.prevent="calculate">
                <div>
                    <label for="limit">Limit amount: </label>
                    <input
                        type="number"
                        name="limit"
                        v-model="limit"
                        required
                        class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100 w-full"
                    />
                </div>
                <div class="py-3">
                    <label for="rate">Currency rate: </label>
                    <input
                        type="number"
                        required
                        v-model="rate"
                        class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100 w-full"
                    />
                </div>

                <div class="py-3">
                    <label for="rate">Bank Percentage: </label>
                    <input
                        type="number"
                        required
                        v-model.number="bankPercentage"
                        class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100 w-full"
                    />
                </div>
                <div class="border-2 rounded-lg px-1 py-3 mb-3">
                    <div class="flex justify-around">
                        <div>
                            <input type="radio" v-model="via" value="ship" />
                            <label for="ship"> By Ship</label>
                        </div>
                        <div>
                            <input type="radio" v-model="via" value="truck" />
                            <label for="truck"> By Truck or Lorry</label>
                        </div>
                    </div>
                    <div v-if="via == 'ship'">
                        <div class="flex flex-col py-2">
                            <label>Condition of Cover: </label>
                            <div class="flex">
                                <select
                                    class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                    v-model="conditionCover"
                                    @change="shipConditionCoverChange"
                                >
                                    <option value="iccb">ICC "B"</option>
                                    <option value="iccc">ICC "C"</option>
                                </select>
                                <input
                                    step="any"
                                    type="number"
                                    v-model.number="shipMarinRate"
                                    class="border border-transparent ml-2 w-full focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                />
                                <span class="m-1">%</span>
                            </div>
                        </div>
                        <div class="py-3">
                            <input
                                class="text-red-500 rounded cursor-pointer"
                                type="checkbox"
                                v-model="war"
                            />
                            <label for="war"> WAR & SRCC</label>
                        </div>
                    </div>
                    <div v-if="via == 'truck'">
                        <div class="flex flex-col py-2">
                            <label>Condition of Cover: </label>
                            <div class="flex">
                                <select
                                    class="border border-transparent focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                    v-model="conditionCover"
                                >
                                    <option value="risk">Risk only</option>
                                </select>
                                <input
                                    step="any"
                                    type="number"
                                    v-model.number="truckMarinRate"
                                    class="border border-transparent ml-2 w-full focus:outline-none focus:ring-2 focus:ring-red-500 focus:border-transparent rounded-lg bg-gray-300 focus:bg-gray-100"
                                />
                                <span class="m-1">%</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="py-3">
                    <input
                        class="text-red-500 rounded cursor-pointer"
                        type="checkbox"
                        v-model="hasStamp"
                    />
                    <label for="hasStamp">
                        Include Stamp Duty
                    </label>
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
                Stamp duty:
                <span class="text-red-500"
                    >{{ stamp | roundFig | numberWithCommas }}
                    <span class="text-sm " v-if="!hasStamp"
                        >Stamp Duty not included</span
                    ></span
                >
            </p>
            <p>
                Total:
                <span class="text-red-500">{{
                    total | roundFig | numberWithCommas
                }}</span>
            </p>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        rate: 85,
        limit: null,
        shipMarinRate: 0.3,
        truckMarinRate: 0.43,
        bankPercentage: 10,
        via: "ship",
        conditionCover: "iccc",
        war: true,
        hasStamp: true,
        netPremium: 0,
        vat: 0,
        stamp: 0,
        total: 0,
    }),
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
        // Condition cover change for #Ship
        shipConditionCoverChange() {
            if (this.conditionCover == "iccc") {
                this.shipMarinRate = 0.3;
            } else if (this.conditionCover == "iccb") {
                this.shipMarinRate = 0.45;
            }
        },
        calculate() {
            this.netPremium = 0;
            this.vat = 0;
            this.stamp = 0;
            this.total = 0;
            let amount;
            parseInt(amount);
            amount = parseInt(this.rate) * parseInt(this.limit);
            if (this.bankPercentage) {
                amount = amount + (amount * this.bankPercentage) / 100;
            }
            // For ship
            if (this.via == "ship") {
                if (this.shipMarinRate && this.limit) {
                    this.netPremium = amount * (this.shipMarinRate / 100);
                }
                if (this.war) {
                    this.netPremium = this.netPremium + amount * (0.05 / 100);
                }
            }
            // For Truck
            if (this.via == "truck") {
                if (this.conditionCover == "risk") {
                    this.netPremium = amount * (this.truckMarinRate / 100);
                }
            }
            // Minimum Net Premium
            if (this.netPremium < 500) {
                this.netPremium = 500;
            }
            // STAMP
            if (this.hasStamp) {
                if (this.via == "ship") {
                    this.stamp = amount / 1500;
                }
                if (this.via == "truck") {
                    this.stamp = 50;
                }
            }
            // VAT
            this.vat = this.netPremium * (15 / 100);
            // TOTAL
            this.total = this.netPremium + this.vat + this.stamp;
        },
    },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Baloo+Da+2&display=swap");
.font-bangla {
    font-family: "Baloo Da 2", cursive;
}
</style>
