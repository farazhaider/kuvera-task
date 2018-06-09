<template>
    <div>
        <div class="px-3  pb-md-1 mx-auto text-center">
            <h1 class="display-4">Mutual Fund Returns</h1>
            <p class="lead">Add your purchases to calculate returns.</p>
        </div>

        <div class="row d-flex justify-content-around">
            <div class="col-sm-6 card bg-light">
                <div class="card-body">
                    <form>
                        <div class="form-group">
                            <label for="investmentAmount">Investment Amount</label>
                            <input type="number" class="form-control" v-model="purchaseRecord.amount" id="investmentAmount" placeholder="Enter the amount you invested.">
                        </div>

                        <div class="form-group">
                            <label for="investmentDate">Investment Date</label>
                            <input type="date" class="form-control" v-model="purchaseRecord.date" id="investmentDate" placeholder="Enter the date you invested on.">
                        </div>

                        <div class="form-group">
                            <label for="fundHouseSelect">Select a fund house</label>
                            <select class="form-control" v-model="purchaseRecord.selectedFundHouse" id="fundHouseSelect">
                                <option v-for="option in options" v-bind:value="option.value">
                                    {{ option.text }}
                                </option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label for="fundSelect">Select a fund</label>
                            <select class="form-control" v-model="purchaseRecord.selectedFund" id="fundSelect">
                                <option v-for="option in options" v-bind:value="option.value">
                                    {{ option.text }}
                                </option>
                            </select>
                        </div>

                        <button type="button" class="btn btn-primary">Compute Return</button>

                    </form>
                </div>
            </div>

            <div class="col-sm-3 card bg-light">
                <div class="card-body">
                    Your Purchase Records
                    <button type="button" class="btn btn-primary">Add a new record</button>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                purchaseRecord : {
                    date: undefined,
                    amount: undefined,
                    selectedFundHouse: '',
                    selectedFund: '',
                },
                options: [
                    { text: 'One', value: 'A' },
                    { text: 'Two', value: 'B' },
                    { text: 'Three', value: 'C' }
                ]
            }
        },
        mounted: function () {
            this.getMutualFundHousesList();
        },
        methods: {
            getMutualFundHousesList() {
                this.$http.get(api).then((response) => {
                    console.log(response.data)
                });
            },
            getMutualFundsList(){
                this.$http.get(api).then((response) => {
                    console.log(response.data)
                });
            },
            computeReturn() {
                this.$http.post(api).then((response) => {
                    console.log(response.data)
                });
            }
        }
    }
</script>

<style>
</style>