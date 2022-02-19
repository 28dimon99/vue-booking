<template>
    <v-container>
        <v-row>
            <v-col lg="8" offset="2">
                <v-stepper v-model="e1">
                    <v-stepper-header>
                        <v-stepper-step :complete="e1 > 1" step="1">Choose destination</v-stepper-step>

                        <v-divider></v-divider>

                        <v-stepper-step :complete="e1 > 2" step="2">Choose flight</v-stepper-step>

                        <v-divider></v-divider>

                        <v-stepper-step step="3">Final information</v-stepper-step>

                    </v-stepper-header>


                    <v-stepper-items>
                        <v-stepper-content step="1">
                            <v-form ref="destinationForm">
                                <v-autocomplete
                                        class="mt-3"
                                        v-model="destination"
                                        :items="destinations"
                                        rounded
                                        outlined
                                        chips
                                        small-chips
                                        dense
                                        label="Destination"
                                        :rules="destinationRules"
                                />

                                <v-row>
                                    <v-col>
                                        <v-text-field label="Start" outlined rounded dense redonly
                                                      @click="dateDialog = true"
                                                      :value="dates[0]"
                                                      :rules="dateStartRules"
                                        />
                                    </v-col>
                                </v-row>
                                <v-row>
                                    <v-col>
                                        <v-text-field label="End" outlined rounded dense redonly
                                                      @click="dateDialog = true"
                                                      :value="dates[1]"
                                                      :rules="dateEndRules"
                                        />
                                    </v-col>
                                </v-row>
                                <v-btn
                                        color="primary"
                                        block
                                        rounded
                                        @click="search"
                                >Search flights
                                </v-btn>
                            </v-form>
                            <v-dialog v-model="dateDialog" width="500">
                                <v-card>
                                    <div class="d-flex flex-column" >
                                        <v-date-picker v-model="dates" range></v-date-picker>
                                        <v-btn @click="dateDialog = false" rounded color="primary">Submit</v-btn>
                                    </div>

                                </v-card>

                            </v-dialog>

                        </v-stepper-content>

                        <v-stepper-content step="2">
                            <v-list class="list_flights">
                                        <v-list-item two-line v-for="item in flightsList" :key="item" @click="flightId = item.id">
                                        <v-list-item-avatar>
                                            <v-img :src="item.image"></v-img>
                                        </v-list-item-avatar>
                                        <v-list-item-content>
                                            <v-list-item-title>{{item.title}}</v-list-item-title>
                                            <v-list-item-subtitle>{{item.dates[0]}} - {{item.dates[1]}}</v-list-item-subtitle>
                                        </v-list-item-content>
                                            <v-list-item-action v-if="item.id === flightId">
                                               <v-chip color="primary">Selected</v-chip>
                                            </v-list-item-action>
                                        </v-list-item>

                            </v-list>
                          <v-row>
                              <v-col>
                                  <v-btn
                                          rounded outlined dense
                                          color="accent" block
                                          @click="e1 = 1"
                                  >
                                      Cancel
                                  </v-btn>
                              </v-col>
                              <v-col>
                                  <v-btn rounded dense
                                         color="primary" block
                                         :disabled="flightId === null"
                                         @click="e1 = 3"
                                  >
                                      Search
                                  </v-btn>
                              </v-col>
                          </v-row>
                        </v-stepper-content>

                        <v-stepper-content step="3">
                            <v-card
                                    class="mb-12"
                                    v-if="selectedFlights"
                            >
                                <v-img  :src="selectedFlights.image">
                                    <v-card-title >{{selectedFlights.title}}</v-card-title>
                                </v-img>
                                <v-card-text>
                                    <div class="subtitle-1">
                                        {{selectedFlights.dates[0]}} - {{selectedFlights.dates[1]}}
                                    </div>
                                </v-card-text>

                            </v-card>

                            <v-row>
                                <v-col>
                                    <v-btn
                                            rounded outlined dense
                                            color="accent" block
                                            @click="e1 = 2"
                                    >
                                        Cancel
                                    </v-btn>
                                </v-col>
                                <v-col>
                                    <v-btn rounded dense
                                           color="primary" block
                                           :disabled="flightId === null"

                                    >
                                        Confirm
                                    </v-btn>
                                </v-col>
                            </v-row>
                        </v-stepper-content>
                    </v-stepper-items>
                </v-stepper>
            </v-col>
        </v-row>


    </v-container>

</template>

<script>

import {fakerList} from '../helpers/fakeData'
    export default {
        name: "Booking",
        data() {
            return {
                e1: 2,
                dates: [],
                dateDialog: false,
                destinations: ['Mars', 'Moon'],
                flightId: null,
                destination: null,
                destinationRules: [v => !!v || 'Destination is required'],
                dateStartRules: [v => !!v || 'Start date is required'],
                dateEndRules: [v => !!v || 'End date is required'],

            }
        },
        methods: {
            search() {
                const isValid = this.$refs['destinationForm'].validate()
                if (!isValid) {
                    return;
                }
                this.e1 = 2
            }
        },
        computed:{
            flightsList(){
                return fakerList(['2022-01-20', '2022-01-22','2022-02-01'], 100)
            },
            selectedFlights(){
                return this.flightsList[this.flightId]
            }
        }
    }
</script>

<style scoped>
    .list_flights{
        height: 60vh;
        overflow-y: auto;

    }

</style>