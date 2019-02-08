<template>
  <section class="container">
    <div class="settings">
      <br>
      <vs-row vs-justify="center">
        <vs-chip 
          closable 
          color="#6ece51" 
          close-icon="assignment" 
          @click="copyToClipboard">
          <vs-avatar
            :src="blocky" 
            size="30px"/>
          {{ address }}
        </vs-chip>
      </vs-row>
      <vs-tabs>
        <vs-tab vs-label="My Wallet">
          <div>
            <vs-button
              color="success"
              type="gradient"
              icon="account_balance_wallet"
            >Get Wallet Balance</vs-button>
            <vs-chip color="success">
              <vs-avatar text="Ξ"/>
              {{ ethBalance }}
            </vs-chip>
          </div>
        </vs-tab>
        <vs-tab vs-label="Open Orders">
          <div>
            <instant/>
          </div>
        </vs-tab>
        <vs-tab vs-label="Trade History">
          <div>Trade History</div>
        </vs-tab>
        <vs-tab 
          disabled 
          vs-label="Disabled">
          <div>Offers</div>
        </vs-tab>
      </vs-tabs>
    </div>
  </section>
</template>
<script>
import { BigNumber, ERC721TokenWrapper, ContractWrappers } from '0x.js'
import { Web3Wrapper } from '@0x/web3-wrapper'
import { toDataUrl } from 'myetherwallet-blockies'
// import { ContractWrappers } from '@0x/contract-wrappers'
import {
  MetamaskSubprovider,
  SignerSubprovider,
  RPCSubprovider,
  Web3ProviderEngine
} from '@0x/subproviders'
// import { ERC721Validator } from '@0xcert/erc721-validator'
import { DummyERC721TokenContract } from '@0x/abi-gen-wrappers'
import { DummyERC721Token } from '@0x/contract-artifacts'

import * as Web3 from 'web3'
import Instant from '@/components/Instant'

export default {
  name: 'Settings',
  components: {
    Instant
  },
  data: function() {
    return {
      web3Wrapper: null,
      contractWrappers: null,
      ethBalance: 0,
      address: '',
      blocky: null
    }
  },
  mounted: function() {
    this.getWalletInfo()
  },
  methods: {
    copyToClipboard() {
      navigator.clipboard.writeText(this.address).then(
        function() {
          this.$vs.notify({
            text: 'Copied to clipboard!',
            color: 'success'
          })
        }.bind(this),
        function() {
          this.$vs.notify({
            text: 'Copy to clipboard failed!',
            color: 'warning'
          })
        }.bind(this)
      )
    },
    async getWalletInfo() {
      const injectedProviderIfExists = window.web3.currentProvider
      const networkId = await new Web3Wrapper(
        injectedProviderIfExists
      ).getNetworkIdAsync()
      const signerProvider =
        injectedProviderIfExists.isMetaMask || injectedProviderIfExists.isToshi
          ? new MetamaskSubprovider(injectedProviderIfExists)
          : new SignerSubprovider(injectedProviderIfExists)
      const provider = new Web3ProviderEngine()
      provider.addProvider(signerProvider)
      provider.addProvider(new RPCSubprovider('https://rinkeby.infura.io'))
      provider.start()
      this.web3Wrapper = new Web3Wrapper(provider)
      this.contractWrappers = new ContractWrappers(provider, { networkId })
      const accounts = await this.web3Wrapper.getAvailableAddressesAsync()
      console.log(accounts)
      const addresses = await this.web3Wrapper.getAvailableAddressesAsync()
      this.address = addresses[0]
      console.log(
        'Owner address: ' + this.address + ' at network: ' + networkId
      )
      const weiBalance = await this.web3Wrapper.getBalanceInWeiAsync(
        this.address
      )
      this.ethBalance = weiBalance.toString()
      console.log('Eth balance: ' + weiBalance)

      this.blocky = toDataUrl(this.address)
      const web3 = new Web3(
        new Web3.providers.HttpProvider('https://rinkeby.infura.io')
      )
      /* Portis is awesome for non-metamask users 
    if (typeof web3 !== 'undefined') {
      // Use Mist/MetaMask's provider
      web3js = new Web3(web3.currentProvider);
      } else {
      // Fallback - use Portis
      web3js = new Web3(new PortisProvider({
      apiKey: 'YOUR_DAPP_API_KEY'
      }));
    }
    */

      //const validator = new ERC721Validator(web3)
      //console.log(
      //  await validator.basic(1, '0x07f96aa816c1f244cbc6ef114bb2b023ba54a2eb') // 0x8f8221afbb33998d8584a2b05749ba73c37a938a')
      //)

      const dummyERC721TokenContracts = []

      for (const tokenAddress of [
        '0x07f96aa816c1f244cbc6ef114bb2b023ba54a2eb'
      ]) {
        dummyERC721TokenContracts.push(
          new DummyERC721TokenContract(
            DummyERC721Token.compilerOutput.abi,
            tokenAddress,
            providerEngine
          )
        )
      }

      const contractAddresses = getContractAddressesForNetworkOrThrow({
        rpcUrl: 'http://127.0.0.1:7545',
        networkId: 5777
      })
      //ifNetworkId != 1 Give error
      // Fetch all the Balances for all of the tokens
      // const allBalancesAsync = tokens.map(token => {
      try {
        const balance = await this.contractWrappers.erc721Token.getTokenCountAsync(
          //use the ERC721 tokenwrapper from 0x directly?
          '0x07f96aa816c1f244cbc6ef114bb2b023ba54a2eb', //'0x8f8221afbb33998d8584a2b05749ba73c37a938a', //0xf176d7bcdD07f8e474877095870685Ef0CCcCb2D
          this.address
        )
        console.log('NFT token balance: ' + balance)
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}
</style>
