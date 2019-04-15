<template>
    <v-app>
        <v-content>
            <v-container>
                <v-layout justify-center>
                    <v-flex xs12 md6 lg4>
                        <Calc/>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-content>
    </v-app>
</template>

<script>
    import Calc from './components/Calc'

    export default {
        name: 'App',
        components: {
            Calc
        },
        data() {
            return {
                //
            }
        }
    }
</script>
