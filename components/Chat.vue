<template>
    <div class="">
        <div class="flex justify-center gap-20 pt-20">
            <div class="border h-[500px] w-[300px] relative">
                <div class="border flex justify-center">
                    Person A
                </div>
                <div class="border w-full overflow-y-auto">
                    <div v-for="(i, k) of chatA" :key="k" class="border overflow-x-auto">
                        {{i}}
                    </div>
                </div>
                <div class="border w-full absolute bottom-0 flex justify-between">
                    <div class="border w-full">
                        <input type="text" v-model="dataA" placeholder="Type here..." @input="inputEventA"
                            class="w-full" />
                    </div>
                    <div class="border cursor-pointer" @click="sendButtonA">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path strokeLinecap="round" strokeLinejoin="round"
                                d="M6 12L3.269 3.126A59.768 59.768 0 0121.485 12 59.77 59.77 0 013.27 20.876L5.999 12zm0 0h7.5" />
                        </svg>
                    </div>
                </div>
            </div>
            <div class="border h-[500px] w-[300px]">
                <div class="border flex justify-center">
                    Hacker
                </div>
                <div class="border w-full overflow-y-auto">
                    <div v-for="(i, k) of chatServer" :key="k" class="border overflow-x-auto">
                        {{i}}
                    </div>
                </div>
            </div>
            <div class="border h-[500px] w-[300px] relative">
                <div class="border flex justify-center">
                    Person B
                </div>
                <div class="border w-full overflow-y-auto">
                    <div v-for="(i, k) of chatB" :key="k" class="overflow-x-auto">
                        {{i}}
                    </div>
                </div>
                <div class="border w-full absolute bottom-0 flex justify-between">
                    <div class="border w-full">
                        <input type="text" v-model="dataB" placeholder="Type here..." @input="inputEventB"
                            class="w-full" />
                    </div>
                    <div class="border cursor-pointer" @click="sendButtonB">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path strokeLinecap="round" strokeLinejoin="round"
                                d="M6 12L3.269 3.126A59.768 59.768 0 0121.485 12 59.77 59.77 0 013.27 20.876L5.999 12zm0 0h7.5" />
                        </svg>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
const NodeRSA = require('node-rsa');
const keyA = new NodeRSA({ b: 512 });
const keyB = new NodeRSA({ b: 512 });
export default {
    // props: {
    //     // data: {
    //     //     required: false,
    //     //     type: Array,
    //     // }
    // },
    data() {
        return {
            dataA: null,
            dataB: null,
            chatA: [],
            chatB: [],
            chatServerA: null,
            chatServerB: null,
            chatServer: [],
            // keyOfA: null,
            // keyOfB: null,
            A: false,
            B: false,
        }
    },
    mounted: () => {
        // const keyA = new NodeRSA({ b: 512 });
        // const keyB = new NodeRSA({ b: 512 });
        // const publicKeyA = keyA.exportKey("public");
        // const privateKeyA = keyA.exportKey("private");
        // const publicKeyB = keyB.exportKey("public");
        // const privateKeyB = keyB.exportKey("private");
        // console.log("publicKeyA: ", typeof(publicKeyA));
        // console.log("privateKeyA: ", privateKeyA);
        // console.log("publicKeyB: ", publicKeyB);
        // console.log("privateKeyB: ", privateKeyB);
        // this.keyOfA = keyA;
        // this.keyOfB = keyB;
        // console.log("keyA: ", keyA);
        // const textA = 'Hello RSA!';
        // const encryptedA = publicKeyA.encrypt(textA, 'base64');
        // console.log('encryptedA: ', encryptedA);
        // const decryptedA = keyB.decrypt(encryptedA, 'utf8');
        // console.log('decryptedA: ', decryptedA);

        // console.log("keyB: ", keyB);
        // const textB = 'Hello RSA!';
        // const encryptedB = keyB.encrypt(textB, 'base64');
        // console.log('encryptedB: ', encryptedB);
        // const decryptedB = keyB.decrypt(encryptedB, 'utf8');
        // console.log('decryptedB: ', decryptedB);
    },


    methods: {
        // blank (data) {
        //     return !data.trim().length;
        // },
        inputEventA(event) {
            this.dataA = event.target.value;
            // console.log("inp: ", this.dataA);
        },
        inputEventB(event) {
            this.dataB = event.target.value;
        },
        receivedByReceiver() {
            if (this.A) {
                const decrypted = keyA.decrypt(this.chatServerA, 'utf8');
                this.chatA.push(decrypted);
                // console.log("decrypted: ", decrypted);
                this.A = false;
                this.$nuxt.refresh()
                // return decrypted
            } else if (this.B) {
                const decrypted = keyB.decrypt(this.chatServerB, 'utf8');
                this.chatB.push(decrypted);
                console.log("decrypted: ", decrypted);
                this.B = false;
                // this.$nuxt.refresh()
                // return decrypted
            }
            // this.$nuxt.refresh()
        },
        sendButtonA() {
            this.chatA.push(this.dataA);
            const encrypted = keyB.encrypt(this.dataA, 'base64');
            this.chatServer.push(encrypted);
            this.chatServerB = encrypted;
            this.receivedByReceiver();
            // this.chatB.push(encrypted);
            this.B = true;
            this.dataA = null;
        },
        sendButtonB() {
            this.chatB.push(this.dataB);
            const encrypted = keyA.encrypt(this.dataB, 'base64');
            this.chatServer.push(encrypted);
            this.chatServerA = encrypted;
            this.receivedByReceiver();
            // this.chatA.push(encrypted);
            this.A = true;
            this.dataB = null;
        },
        // decryptA(data) {
        //     if (this.A) {
        //         const decrypted = keyA.decrypt(data, 'utf8');
        //         console.log("decrypted: ", decrypted);
        //         this.A = false;
        //         return decrypted
        //     } else {
        //         return data
        //     }
        // },
        // decryptB(data) {
        //     // console.log("dataB: ", data);
        //     // console.log("B: ", this.B);
        //     if (this.B) {
        //         const decrypted = keyB.decrypt(data, 'utf8');
        //         console.log("decrypted: ", decrypted);
        //         this.B = false;
        //         return decrypted
        //     } else {
        //         return data
        //     }
        // }
    },
}
</script>