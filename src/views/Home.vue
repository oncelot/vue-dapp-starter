<template>
    <div style="padding-top:20px">
        <div class="flex flex-row">
            <input type="text" v-model="addressSend" placeholder="Address" style="margin:auto" />
        </div>
        <div class="flex flex-row">
            <input type="text" v-model="valueSend" placeholder="Matic" style="margin:auto" />
        </div>
        <div class="flex flex-row">
            <button class="btn btn-teal" style="margin:auto">Send</button>
        </div>
    </div>
    <div class="sm:container sm:mx-auto" style="max-width: 650px">
        <div class="flex flex-row" style="padding-top:20px">
            <button
                style="margin:auto"
                class="btn btn-teal"
                @click="bottonSwitch()"
                v-if="!chainIdOk"
            >Switch in Polygon/Matic</button>
        </div>

    </div>
</template>

<script>

import { ethers } from "ethers";


export default ({
    data() {
        return {
            addressSend: '', valueSend: '', chainIdOk: true,
            chainId: "0x13881",
            chainName: "Mumbai",
            rpcUrls: "https://rpc-mumbai.matic.today",
            blockExplorerUrls: "https://explorer-mumbai.maticvigil.com",
            //80001
        }
    },
    methods: {
        send() {
            windows.ethereum.request({
                method: 'eth_sendTransaction',
                params: [{
                    from: accounts[0],
                    to: this.addressSend,
                    value: ethers.utils.parseEther("" + this.valueSend),
                },],
            }).then((txHash) => console.log(txHash)).catch((error) => console.error);
        },
        bottonSwitch() {
            //window.ethereum.SwitchEthereumChainParameter({chainId:'https://polygon-rpc.com'})
            window.ethereum
                .request({ method: "eth_requestAccounts" })
                .then((response) => {
                    window.ethereum.request({
                        method: 'wallet_switchEthereumChain',
                        params: [{ chainId: this.chainId }]
                    })
                        .then((response) => { })
                        .catch(() => {
                            // alert('alert Add Wallet Polygon');
                            window.ethereum.request({
                                method: "wallet_addEthereumChain",
                                params: [
                                    {
                                        chainId: this.chainId,
                                        chainName: this.chainName,
                                        rpcUrls: [this.rpcUrls],
                                        nativeCurrency: {
                                            name: "Matic",
                                            symbol: "Matic",
                                            decimals: 18,
                                        },
                                        blockExplorerUrls: [this.blockExplorerUrls],
                                    },
                                ],
                            });
                        })
                })
        },
    },
    mounted() {
        let provider2 = new ethers.providers.Web3Provider(window.ethereum);

        provider2.getNetwork().then((ris) => {

            console.log(ris.chainId);
            console.log(this.chainPolygon);
            if (ris.chainId === this.chainPolygon) { this.chainIdOk = true; } else { this.chainIdOk = false; }
        });
    }
})

//if Chain change metamask
try {
    window.ethereum.on('chainChanged', (_chainId) => window.location.reload());
    }
     catch { }
</script>

<style>
.btn {
    @apply font-bold py-1 px-2 rounded;
}
.btn-teal {
    @apply bg-teal-500 text-white;
}
.btn-teal:hover {
    @apply bg-teal-700;
}
</style>
