<template>
    <v-container>
        <v-card>
            <v-layout row>
                <v-text-field v-model="inputText" :suffix="solution.length > 0 && solution !== 'NaN' ? '=' + solution : ''"
                              solo></v-text-field>
                <h1>{{ }}</h1>
            </v-layout>
        </v-card>
    </v-container>
</template>

<script>
    export default {
        data: () => ({
            inputText: "",
            solution: "0",
        }),
        watch: {
            inputText(inputText) {
                console.log(inputText);
                this.solution = this.compute(inputText);
            }
        },
        methods: {
            compute(str) {
                function isAction(str) {
                    return str === "+" || str === "-" || str === "/" || str === "*";
                }

                let list = str.split(/([+\-/*?]\w*)/).filter(value => value.length > 0);

                console.log(list);

                let action, number, finalResult = "0";
                for (let str of list) {
                    if (isAction(str[0])) {
                        action = str[0];
                        number = str.substr(1);
                    } else {
                        action = null;
                        number = str;
                    }
                    number = parseInt(number);

                    console.log('action:', action);
                    console.log('num:', number);

                    let result = parseInt(finalResult);
                    console.log("result before:", finalResult," to ", result);

                    switch (action) {
                        case '-':
                            result -= number;
                            break;
                        case '+':
                            result += number;
                            break;
                        case '/':
                            result /= number;
                            break;
                        case '*':
                            result *= number;
                            break;
                        default:
                            result += number;
                            break;
                    }
                    finalResult = result.toString();
                    console.log('result after: ', result.toString(), ' to ', finalResult);
                }


                console.log('---------', finalResult);

                return finalResult;
            }
        },
    }
</script>

<style>
    html {

    }
</style>
