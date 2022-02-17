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
                          <v-row>
                              <v-col>
                                  <v-btn rounded outlined dense color="accent" block>Cancel</v-btn>
                              </v-col>
                              <v-col>
                                  <v-btn rounded dense color="primary" block>Search</v-btn>
                              </v-col>
                          </v-row>
                        </v-stepper-content>

                        <v-stepper-content step="3">
                            <v-card
                                    class="mb-12"
                                    color="grey lighten-1"
                                    height="200px"
                            ></v-card>

                            <v-btn
                                    color="primary"
                                    @click="e1 = 1"
                            >
                                Continue
                            </v-btn>

                            <v-btn text>
                                Cancel
                            </v-btn>
                        </v-stepper-content>
                    </v-stepper-items>
                </v-stepper>
            </v-col>
        </v-row>


    </v-container>

</template>

<script>
    export default {
        name: "Booking",
        data() {
            return {
                e1: 2,
                dates: [],
                dateDialog: false,
                destinations: ['Mars', 'Moon'],
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
        }
    }
</script>

<style scoped>

</style>