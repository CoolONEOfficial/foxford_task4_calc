<template>
    <v-container>
        <v-card>
            <v-layout row wrap>
                <v-flex xs12 style="height: 50px;">
                    <v-text-field
                            style="position: absolute; width: 100%;"
                            v-model="inputText"
                            :suffix="solution.length > 0 && solution !== 'NaN' ? '=' + solution : ''"
                            solo></v-text-field>
                </v-flex>

                <v-flex xs3 v-for='item of [
                    ["1", "1"],
                    ["2", "2"],
                    ["3", "3"],
                    ["+", "+"],

                    ["4", "4"],
                    ["5", "5"],
                    ["6", "6"],
                    ["-", "-"],

                    ["7", "7"],
                    ["8", "8"],
                    ["9", "9"],
                    ["*", "*"],

                    [",", "."],
                    ["0", "0"],
                    ["C", null],
                    ["/", "/"],

                    ["(", "("],
                    [")", ")"],
                    ["SQR", "**"],
                    ["√", "^"],
                ]' :key="item">
                    <v-card v-ripple @click="item[1] != null ?
                        inputText+=item[1]
                        : inputText = ''">
                        <h1 class="display-1 text-xs-center pa-3">{{ item[0] }}</h1>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-card>
    </v-container>
</template>

<script>
    var XRegExp = require('xregexp');

    export default {
        data: () => ({
            inputText: "",
            solution: "0",
        }),
        watch: {
            inputText(inputText) {
                console.log(inputText);
                this.solution = this.compute(inputText).toString();
            }
        },
        methods: {
            compute(str) {

                console.log("--- compute str ", str, " ---");

                let res = str;
                for(let match of XRegExp.matchRecursive(str, '\\(', '\\)', 'g')) {
                    let innerRes = this.compute(match);
                    console.log("match: ", match, " -> ", innerRes.toString());

                    res = res.replace('(' + match + ')', innerRes);
                }

                console.log("replaced: ", res);
                console.log("splitt: ", res.split(/([+\-/^*?]*-?\d*(\.\d+)?)/));
                res = res.split(/([+\-/^*?]*-?\d*(\.\d+)?)/)
                    .filter(value => value !== undefined && value.length > 0 && value[0] !== '.')
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

                console.log("final res: ", res.toString());

                return res.length > 0 ? res[0][0] === "-" ? -res[0][1] : res[0][1] : 0;
            }
        },
    }
</script>

<style>
    html {

    }
</style>
