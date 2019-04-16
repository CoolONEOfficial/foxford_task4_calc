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
                this.solution = this.compute(inputText);
            }
        },
        methods: {
            compute(str) {
                console.log("-------");
                let res = str.split(/([+\-/*?]\w*)/)
                        .filter(value => value.length > 0)
                        .map(value => {
                            // console.log('val', value);

                            let action, number;

                            switch (value[0]) {
                                case "+":
                                case "-":
                                case "*":
                                case "/":
                                    action = value[0];
                                    number = value.substr(1);
                                    break;
                                default:
                                    action = '+';
                                    number = value;
                            }
                            number = parseFloat(number);
                            if (isNaN(number)) number = 0;

                            // console.log('pair: ', action, number);

                            return [action, number];
                        })
                    // .reduce(
                    //     (pre, curr) => {
                    //         console.log("*/ reduce");
                    //         console.log("pre", pre, "curr", curr);
                    //         let number = pre[1];
                    //         if(number === undefined) return pre;
                    //         switch (curr[0]) {
                    //             case "*":
                    //                 number *= curr[1];
                    //                 break;
                    //             case "/":
                    //                 number /= curr[1];
                    //                 break;
                    //             default:
                    //                 console.log("default");
                    //                 return pre;
                    //         }
                    //         console.log("0: ", pre[0], "num: ", number);
                    //         return [pre[0], number];
                    //     }
                    // )
                    // .reduce(
                    //     (pre, curr) => {
                    //         console.log("+- reduce");
                    //         console.log("pre", pre, "curr", curr);
                    //         let number = pre[1];
                    //         if(number === undefined) return pre;
                    //         switch (curr[0]) {
                    //             case "+":
                    //                 number += curr[1];
                    //                 break;
                    //             case "-":
                    //                 number -= curr[1];
                    //                 break;
                    //             default:
                    //                 return pre;
                    //         }
                    //         console.log("0: ", pre[0], "num: ", number);
                    //         return [pre[0], number];
                    //     }
                    // )
                ;

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
