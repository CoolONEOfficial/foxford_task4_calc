<template>
    <v-container>
        <v-card>
            <v-input></v-input>
            <v-layout row>
                <v-text-field v-model="input" outline></v-text-field>
                <h1>{{  }}</h1>
            </v-layout>
        </v-card>
    </v-container>
</template>

<script>
    export default {
        data: () => ({
            input: "",
            solution: "",
            watch: {
                input() {
                    let list = this.input.split(/[0-9]*[+|\-\\*/]?/);

                    let pair;
                    for(let str in list) {
                        pair = [str.slice(0, str.length - 2), str[str.length - 1]];


                    }
                }
            },
            methods: {
                compute(str) {

                }
            },
        }),
    }
</script>

<style>

</style>
