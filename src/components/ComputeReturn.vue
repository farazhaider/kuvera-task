<template>
    <div>
        <div class="px-3  pb-md-1 mx-auto text-center">
            <h1 class="display-4">Mutual Fund Returns</h1>
            <p class="lead">Add your purchases to calculate returns.</p>
        </div>

        <div class="row d-flex justify-content-around">
            <div class="col-sm-4 card bg-light">
                <div class="card-body">
                    <form>
                        <div class="form-group">
                            <label for="investmentAmount">Investment Amount</label>
                            <input type="number" class="form-control" v-model="purchaseRecord.amount" id="investmentAmount" placeholder="Enter the amount you invested.">
                        </div>

                        <div class="form-group">
                            <label for="investmentDate">Investment Date</label>
                            <datepicker name="uniquename" v-model="purchaseRecord.date" id="investmentDate"></datepicker>
                        </div>

                        <div class="form-group">
                            <label for="fundHouseSelect">Select a fund house</label>
                            <select class="form-control" v-model="purchaseRecord.selectedFundHouse" v-on:change="onFundHouseSelected" id="fundHouseSelect">
                                <option v-for="option in mfHouseOptions" v-bind:value="option">
                                    {{ option }}
                                </option>
                            </select>
                        </div>

                        <div class="form-group" v-if="purchaseRecord.selectedFundHouse">
                            <label for="fundSelect">Select a fund</label>
                            <select class="form-control" v-model="purchaseRecord.selectedFund" id="fundSelect">
                                <option v-for="option in mfOptions" v-bind:value="option">
                                    {{ option }}
                                </option>
                            </select>
                        </div>

                        <button type="button" class="btn btn-primary" v-on:click="computeReturn">Compute Return</button>

                        <div class="alert alert-success my-2" v-if="purchaseRecord.returnAmount" role="alert">Your return amount is {{}}</div>

                    </form>
                </div>
            </div>

            <div class="col-sm-7 card bg-light">
                <div class="card-body">
                    Your Purchase Records
                    <table class="table table-sm table-striped my-2">
                        <thead>
                            <tr>
                                <th scope="col">Fund House</th>
                                <th scope="col text-truncate">Mutual Fund</th>
                                <th scope="col">Amount Invested</th>
                                <th scope="col">Return Amount</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="purchase in purchaseRecords">
                                <td>{{purchase.selectedFundHouse}}</td>
                                <td>{{purchase.selectedFund}}</td>
                                <td>{{purchase.amount}}</td>
                                <td>{{purchase.returnAmount}}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

        </div>

    </div>
</template>

<script>
    import Datepicker from 'vuejs-datepicker';
    export default {
        components: {
            Datepicker
        },
        data: function () {
            return {
                purchaseRecord: {
                    date: undefined,
                    amount: undefined,
                    selectedFundHouse: '',
                    selectedFund: '',
                    returnAmount: ''
                },
                purchaseRecords: [

                ],
                mfHouseOptions: [
                ],
                mfOptions: [

                ]
            }
        },
        mounted: function () {
            this.getMutualFundHousesList();
            console.log(this.$urls);
        },
        methods: {
            getMutualFundHousesList() {
                this.$http.get(this.$urls.BASE_URL+this.$urls.GET_MUTUAL_FUND_HOUSES).then((response) => {
                    this.mfHouseOptions = response.data;
                })
                .catch((error) => {
                    alert(error.message);
                });
            },
            onFundHouseSelected() {
                this.getMutualFundsList();
            },
            getMutualFundsList() {
                this.$http.get(this.$urls.BASE_URL+this.$urls.GET_MUTUAL_FUNDS, {
                    params: {
                        selectedFundHouse: this.purchaseRecord.selectedFundHouse
                    }
                }).then((response) => {
                    this.mfOptions = response.data;
                })
                .catch((error) => {
                    alert(error.message);
                });
            },
            computeReturn() {
                this.$http.post(this.$urls.BASE_URL+this.$urls.COMPUTE_RETURN_VALUE, this.purchaseRecord).then((response) => {
                    this.purchaseRecord.returnAmount = response.data.returnAmount;
                    this.purchaseRecords.push(this.purchaseRecord);
                    this.clearPurchaseRecordForm();
                })
                .catch((error) => {
                    alert(error.message);
                });
            },
            clearPurchaseRecordForm() {
                this.purchaseRecord = {
                    date: undefined,
                    amount: undefined,
                    selectedFundHouse: '',
                    selectedFund: '',
                    returnAmount: ''
                };
            }
        }
    }
</script>

<style>
</style>