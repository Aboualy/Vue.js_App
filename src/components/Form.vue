<template>

    <div>


        <fieldset class = "split left">

                            <form @submit.prevent="validateUser">
                                <md-card-header>
                                    <div class="font">Registration Form</div>
                                </md-card-header>
                                <md-card-content>
                                    <md-field :class="getValidationClass('fullName')">
                                        <label for="full-name" class="font">Full Name</label>
                                        <md-input name="full-name" id="full-name" autocomplete="given-name" v-model="form.fullName" :disabled="sending" />
                                        <span class="md-error" v-if="!$v.form.fullName.required">The full name is required</span>
                                        <span class="md-error" v-else-if="!$v.form.fullName.minlength">Invalid full name</span>
                                    </md-field>

                                    <md-field :class="getValidationClass('carMake')">
                                        <label for="car-make" class="font">Car Make</label>
                                        <md-input name="carMake" id="car-make" v-model="form.carMake" :disabled="sending" />
                                        <span class="md-error" v-if="!$v.form.carMake.required">The car make is required</span>
                                        <span class="md-error" v-else-if="!$v.form.carMake.minlength">Invalid car make </span>
                                    </md-field>

                                    <md-field :class="getValidationClass('carModel')">
                                        <label for="car-model" class="font">Car Model</label>
                                        <md-input name="carModel" id="car-model" v-model="form.carModel" :disabled="sending" />
                                        <span class="md-error" v-if="!$v.form.carModel.required">The car model is required</span>
                                        <span class="md-error" v-else-if="!$v.form.carModel.minlength">Invalid car model </span>
                                    </md-field>

                                    <md-field :class="getValidationClass('extraDes')">
                                        <label for="car-des" class="font">Extra description</label>
                                        <md-textarea name="extraDes" id="car-des" v-model="form.extraDes" :disabled="sending" />
                                        <span class="md-error" v-if="!$v.form.extraDes.required">Extra description is required</span>
                                        <span class="md-error" v-else-if="!$v.form.extraDes.minlength">Invalid extra description </span>
                                    </md-field>

                                </md-card-content>

                                <md-progress-bar md-mode="indeterminate" v-if="sending" />



                                <md-input id="input" :class="getValidationClass('photos')" accept="image/*" multiple type="file" @change="onFileChange" class="input" :disabled="sending"/>

                                <v-btn outline small id="IButton" onclick="document.getElementById('input').click()"
                                       :loading="loading3"
                                       :disabled="loading3"
                                       class="black--text"
                                       @click.native="loader = 'loading3'">upload
                                    <v-icon small right dark>cloud_upload</v-icon></v-btn>

                                <br/>



                                <v-btn outline small flat type="submit" class="black--text" color="black" :disabled="sending">
                                    Sign up<v-icon small right dark>check_circle</v-icon>
                                </v-btn>
                                <md-snackbar :md-active.sync="userSaved">The user {{ lastUser }} was saved with success!</md-snackbar>


                            </form>

    </fieldset>

        <div class="split right ">

               <md-bottom-bar md-theme="black">
                   <md-icon> camera</md-icon>
               </md-bottom-bar>

            <div style='overflow:auto; width:715px;height:375px;'>
            <md-card v-for="u in form.photos" v-bind:key="u.id">
                <md-card-media-cover>
                    <md-card-media md-ratio="16:9">
                        <img :src="u.image" alt="Skyscraper ">
                    </md-card-media>

                    <md-card-area >

                        <md-card-actions>
                            <div>
                                <md-button small class="md-fab md-mini md-accent">
                                    <md-icon>edit</md-icon>
                                </md-button>
                                <md-button small class="md-fab md-mini md-accent">
                                    <md-icon>remove</md-icon>
                                </md-button>
                            </div>
                            <md-card-expand-trigger>
                                <md-button class="md-icon-button md-accent">
                                    <md-icon>keyboard_arrow_down</md-icon>
                                </md-button>
                            </md-card-expand-trigger>
                        </md-card-actions>
                        <md-card-expand-content>
                            <md-card-content class="black--text ">
                                voluptas eius illo quas, saepe voluptate pariatur in deleniti minus sint. Excepturi.
                            </md-card-content>
                        </md-card-expand-content>
                    </md-card-area>
                </md-card-media-cover>
            </md-card>

        </div>
        </div>



    </div>
</template>



<script>
    import { validationMixin } from 'vuelidate'
    import {
        required,
        minLength,

    } from 'vuelidate/lib/validators'
    export default {
        name: "Form",
        mixins: [validationMixin],
        data: () => ({
            form: {
                id:1,
                fullName: null,
                carMake: null,
                carModel: null,
                extraDes: null,
                photos:[],


            },
            errors: [],
            userSaved: false,
            sending: false,
            lastUser: null
        }),
        validations: {
            form: {
                fullName: {
                    required,
                    minLength: minLength(5)
                },
                carMake: {
                    required,
                    minLength: minLength(5)
                },
                carModel: {
                    required,
                    minLength: minLength(5)
                },
                extraDes: {
                    required,
                    minLength: minLength(5)
                },

            }
        },
        methods: {
            onFileChange(e) {

                const files = e.target.files;
                const len = files.length;
                for (let i =0; i < len; i++){
                    const file = URL.createObjectURL(files[i]);
                    this.form.photos.push({id:i, image: file});
                }
            },

            getValidationClass (fieldName) {
                const field = this.$v.form[fieldName]

                if (field) {
                    return {
                        'md-invalid': field.$invalid && field.$dirty
                    }
                }
            },
            clearForm () {
                this.$v.$reset()
                this.form.fullName = null
                this.form.carMake=null
                    this.form.carModel=null
                    this.form.extraDes=null

            },
            saveUser () {
                this.sending = true

                // Instead of this timeout, here you can call your API
                window.setTimeout(() => {
                    this.lastUser = `${this.form.fullName}`
                    this.userSaved = true
                    this.sending = false
                    this.clearForm()
                }, 10)
            },
            validateUser () {
                this.$v.$touch()

                if (!this.$v.$invalid) {
                    this.saveUser()
                }
            }
        }
    }
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    @import './style/Form.css';
</style>
