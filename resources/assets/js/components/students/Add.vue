<template>
    <v-container>
        <v-layout row >
            <v-flex xs12 sm6 offset-sm3>

            </v-flex>
        </v-layout>
        <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
                <v-btn type="button" style="margin-left:0px;" to="/" class="primary">Back</v-btn>
                <v-card>
                    <v-card-text>
                        <v-container>
                            <form @submit.prevent="submit">
                                <v-alert :color="alert_color" :icon="alert_icon" dismissible v-model="alert"> {{ alert_msg }}
                                </v-alert>
                                <v-layout row>
                                    <v-flex xs12>
                                        <v-toolbar-title class="display-1" style="margin:0px;">Add Stuudent</v-toolbar-title>
                                    </v-flex>
                                </v-layout>
                                <v-layout row>
                                    <v-flex xs12>
                                        <v-text-field label="Full Name" name="full_name" id="full_name" v-model="form.full_name" v-validate="'required'" :error-messages="errors.collect('full_name')" required>
                                        </v-text-field>
                                    </v-flex>
                                </v-layout>
                                <v-layout row>
                                    <v-flex xs12>
                                        <v-text-field label="Father Name" name="father_name" id="father_name" v-model="form.father_name" v-validate="'required'"  :error-messages="errors.collect('father_name')" required>
                                        </v-text-field>
                                    </v-flex>
                                </v-layout>
                                <v-layout row>
                                    <v-flex xs12>
                                        <v-text-field label="E-mail" id="email" name="email" v-model="form.email" v-validate="'required|email'" :error-messages="errors.collect('email')" type="email" required>
                                        </v-text-field>
                                    </v-flex>
                                </v-layout>
                                <v-layout row>
                                    <v-flex xs12>
                                        <v-text-field label="Address" name="address" id="address" v-model="form.address" v-validate="'required'"  :error-messages="errors.collect('address')" required>
                                        </v-text-field>
                                    </v-flex>
                                </v-layout>
                                <v-layout row>
                                    <v-flex xs12 sm12>
                                        <v-menu  lazy :close-on-content-click="false" v-model="menu"  transition="scale-transition" offset-y full-width :nudge-right="40" max-width="290px" min-width="290px"
                                        >
                                        <v-text-field slot="activator" label="Birthday" v-model="form.dob" name="dob"  readonly>
                                        </v-text-field>
                                        <v-date-picker v-model="form.dob" >
                                            <template slot-scope="{ save, cancel }">
                                                <v-card-actions>
                                                    <v-spacer></v-spacer>
                                                    <v-btn flat color="primary" @click="cancel">Cancel</v-btn>
                                                    <v-btn flat color="primary" @click="save">OK</v-btn>
                                                </v-card-actions>
                                            </template>
                                        </v-date-picker>
                                    </v-menu>
                                </v-flex>
                            </v-layout>
                            <v-layout row>
                                <v-flex xs12>
                                    <v-btn type="submit" style="margin:0px;" color="primary" :loading="loading" @click.native="loader = 'loading'" :disabled="loading">
                                        Submit
                                    </v-btn>
                                    <v-btn @click="clear">clear</v-btn>
                                </v-flex>
                            </v-layout>
                        </form>
                    </v-container>
                </v-card-text>
            </v-card>
        </v-flex>
    </v-layout>
</v-container>
</template>
<script>
export default {
    data() {
        return {
            alert: false,
            alert_color: '',
            alert_msg: '',
            alert_icon:'',
            menu: false,
            loading: false,
            form:{
                full_name: '',
                email: '',
                father_name: '',
                address: '',
                dob:null,
            }
        }

    },
    methods: {
        submit () {

            this.loading = true;
            this.$validator.validateAll();
            axios.post('/api/student',this.form).then(res =>{
                if(res.status == 200 && res.data.status === "success"){
                    this.alert = true;
                    this.alert_color = "success";
                    this.alert_icon = 'check_circle';
                    this.alert_msg = res.data.message;
                    this.loading = false;
                    this.clear();
                }
            })
            .catch( err => {
                if (err.response) {
                    if(err.response.status == 422){
                        console.log(err.response.data);
                        this.alert = true;
                        this.alert_color = "error";
                        this.alert_msg = 'something went wrong. please try again';
                        this.loading = false;
                    }
                }
            });
        },
        clear () {
            this.form.full_name = '';
            this.form.email = '';
            this.form.father_name = '';
            this.form.address = '';
            this.form.dob = null;
            this.$validator.reset()
        }
    },
}
</script>