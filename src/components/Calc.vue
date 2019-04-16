<template>
    <v-container>
        <v-card>
            <v-layout row>
                <v-text-field v-model="inputText"
                              :suffix="solution.length > 0 && solution !== 'NaN' ? '=' + solution : ''"
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
                this.solution = this.compute(inputText)[0][1].toString();
            }
        },
        methods: {
            compute(str) {
                // let array = [], c = 0;

                // console.log("-------");
                // console.log(str.split(/([()])/).filter(Boolean).forEach(e =>
                //     e === '('
                //         ? c++
                //         : e === ')'
                //         ? c--
                //         : array.push(e)
                // ));
                // console.log(array.toString());

                let res = str
                    .split(/([+\-/^*?]*-?\w*)/)
                    .filter(value => value.length > 0)
                    .map(value => {
                        let action, number;

                        switch (value[0]) {
                            case "+":
                            case "-":
                            case "*":
                            case "/":
                            case "^":
                                if (value[1] === "*") {
                                    action = value.substr(0, 2);
                                    number = value.substr(2);
                                } else {
                                    action = value[0];
                                    number = value.substr(1);
                                }
                                break;
                            default:
                                action = '+';
                                number = value;
                        }
                        number = parseFloat(number);
                        if (isNaN(number)) number = 0;

                        return [action, number];
                    });

                for (let step of Array(2).keys()) {
                    console.log("---step ", step, "---");
                    console.log("res: ", res.toString());

                    let prevPair = res[0];
                    for (let pairId = 1; pairId < res.length; pairId++) {
                        let pair = res[pairId];

                        let found = true;

                        switch (step) {
                            case 0: // */
                                switch (pair[0]) {
                                    case "*":
                                        prevPair[1] *= pair[1];
                                        break;
                                    case "/":
                                        prevPair[1] /= pair[1];
                                        break;
                                    case "^":
                                        prevPair[1] = prevPair[1] ** (1 / pair[1]);
                                        break;
                                    case "**":
                                        prevPair[1] **= pair[1];
                                        break;
                                    default:
                                        found = false;
                                }
                                break;
                            case 1: // +-
                                switch (pair[0]) {
                                    case "+":
                                        prevPair[1] += pair[1];
                                        break;
                                    case "-":
                                        prevPair[1] -= pair[1];
                                        break;
                                    default:
                                        found = false;
                                }
                                break;
                        }

                        if (found) {
                            res.splice(pairId, 1);

                            pairId--;
                        } else prevPair = pair;
                    }
                }

                console.log("final res: ", res);

                return res;
            }
        },
    }
</script>

<style>
    html {

    }
</style>
