<template>
  <section class="container">
    <div class="sell">
      <br>
      <h1>Sell NFT</h1>
      <br>
      <vs-row vs-justify="center">
        <vs-select
          v-model="selectedAddress"
          autocomplete
          class="selectExample"
          label="Select Token"
        >
          <div 
            v-for="(group, key) in tokensNFT"
            :key="group">
            <vs-select-group :title="key">
              <vs-select-item 
                v-for="(token, index) in group"
                :key="index" 
                :value="token.asset_contract.address" 
                :text="'# ' + token.token_id"/>
            </vs-select-group>
          </div>
        </vs-select>
      </vs-row>
      <br>
      <vs-row vs-justify="center">
        <vs-input 
          v-model="tokenName"
          placeholder="Title"/>
      </vs-row>
      <br>
      <vs-row 
        vs-justify="center">
        <vs-col vs-w="4">
          <vs-textarea 
            v-model="tokenDescription" 
            label="Description" />
        </vs-col>
      </vs-row>
      <vs-row vs-justify="center">
        <picture-input 
          ref="pictureInput"
          :custom-strings="{
            upload: '<h1>Bummer!</h1>',
            drag: 'Drag or upload an image here 😺'
          }" 
          width="300" 
          height="300" 
          margin="16" 
          accept="image/jpeg,image/png" 
          size="10"
          @change="onChange"/>
      </vs-row>
      <br>
      <vs-row vs-justify="center">
        <vs-input-number 
          v-model="price"
          min="0"/>
      </vs-row>
      <vs-row vs-justify="center">
        <vs-button 
          type="filled" 
          @click="makerOrder">Sell for Ξ {{ price }}</vs-button>
      </vs-row>
      <br>
    </div>
  </section>
</template>
<script>
// import PictureInput from '@/components/PictureInput/PictureInput'
import {
  assetDataUtils,
  BigNumber,
  ContractWrappers,
  generatePseudoRandomSalt,
  Order,
  orderHashUtils,
  signatureUtils,
  RPCSubprovider,
  MetamaskSubprovider,
  SignerSubprovider,
  Web3ProviderEngine
} from '0x.js'
import { HttpClient, OrderbookRequest, OrderConfigRequest } from '@0x/connect'
import { Web3Wrapper } from '@0x/web3-wrapper'
import { DummyERC721TokenContract } from '@0x/abi-gen-wrappers'
import { getContractAddressesForNetworkOrThrow } from '@0x/contract-addresses'
import { DummyERC721Token } from '@0x/contract-artifacts'

export default {
  name: 'Sell',
  // components: { PictureInput },
  data: function() {
    return {
      tokenName: '',
      tokenDescription: '',
      selectedAddress: null, //'0x13793e8c72f81999fdc89f2731c9a4c895aac2de',
      price: 1,
      web3Wrapper: null,
      contractWrappers: null,
      providerEngine: null,
      ethBalance: 0,
      address: '',
      maker: '',
      taker: '',
      walletNFT: null,
      tokenId: ''
    }
  },
  computed: {
    /*eslint-disable */
    tokensNFT() {
      if (this.walletNFT != null) {
        const tokens = this.walletNFT.assets.reduce(function(r, asset) {
          r[asset.asset_contract.name] = r[asset.asset_contract.name] || []
          r[asset.asset_contract.name].push(asset)
          return r
        }, Object.create(null))

        this.selectedAddress = tokens[Object.keys(tokens)[0]][0].asset_contract.address || null
        return tokens
      }
      // return Object.keys(tokensNFT).forEach(function(token, index) {
      // update name property
      // tokensNFT[token].title = tokensNFT[token]
      // assign object property based on old property value
      // tokensNFT[token] = index
      // })
    }
    /*eslint-enable */
  },
  mounted: async function() {
    await this.getWalletInfo()
    this.mintToken()
  },
  methods: {
    async makerOrder() {
      const selectedAddress = this.selectedAddress
      let tokenId = ''
      const tokensNFT = this.tokensNFT
      //const tokenId = Object.keys(tokensNFT).forEach(function(group, index) {
      //  return tokensNFT[group].find(
      //    token => token.asset_contract.address == selectedAddress
      //  )
      //})

      Object.keys(tokensNFT).forEach(function(group) {
        tokensNFT[group].map(function(token) {
          if (token.asset_contract.address == selectedAddress)
            tokenId = token.token_id
        })
      })

      // Token Addresses
      const contractAddresses = getContractAddressesForNetworkOrThrow(4)
      const zrxTokenAddress = contractAddresses.zrxToken
      const etherTokenAddress = contractAddresses.etherToken
      const DECIMALS = 18

      const exchangeAddress = contractAddresses.exchange
      console.log(tokenId)
      const makerAssetData = assetDataUtils.encodeERC721AssetData(
        this.selectedAddress,
        tokenId
      )
      const takerAssetData = assetDataUtils.encodeERC20AssetData(
        etherTokenAddress
      )
      // the amount the maker is selling of maker asset
      const makerAssetAmount = Web3Wrapper.toBaseUnitAmount(
        new BigNumber(1),
        DECIMALS
      )
      // the amount the maker wants of taker asset
      const takerAssetAmount = Web3Wrapper.toBaseUnitAmount(
        new BigNumber(0.1),
        DECIMALS
      )

      // Allow the 0x ERC721 Proxy to move ERC721 tokens on behalf of maker
      const makerERC721ApprovalTxHash = await this.contractWrappers.erc721Token.setProxyApprovalForAllAsync(
        this.selectedAddress,
        this.maker,
        true
      )
      // await printUtils.awaitTransactionMinedSpinnerAsync('Maker ERC721 Approval', makerERC721ApprovalTxHash);
      await this.web3Wrapper.awaitTransactionSuccessAsync(
        makerERC721ApprovalTxHash
      )

      // Create the order
      const order = {
        exchangeAddress,
        makerAddress: this.maker,
        takerAddress: '0x0000000000000000000000000000000000000000',
        senderAddress: '0x0000000000000000000000000000000000000000',
        feeRecipientAddress: '0x0000000000000000000000000000000000000000',
        expirationTimeSeconds: 6000,
        salt: generatePseudoRandomSalt(),
        makerAssetAmount: 1,
        takerAssetAmount: this.price,
        makerAssetData,
        takerAssetData,
        makerFee: new BigNumber(0),
        takerFee: new BigNumber(0),
        relayerClient: null
      }

      // Generate the order hash and sign it
      const orderHashHex = orderHashUtils.getOrderHashHex(order)
      const signature = await signatureUtils.ecSignHashAsync(
        this.providerEngine,
        orderHashHex,
        this.maker
      )
      const signedOrder = { ...order, signature }

      await this.contractWrappers.exchange.validateOrderFillableOrThrowAsync(
        signedOrder
      )

      await this.relayerClient.submitOrderAsync(signedOrder, { networkId: 4 })

      /*
      this.$vs.loading()
      setTimeout(() => {
        this.$vs.loading.close()
      }, 2000)
      */
    },
    groupBy(list, keyGetter) {
      const map = new Map()
      list.forEach(item => {
        const key = keyGetter(item)
        const collection = map.get(key)
        if (!collection) {
          map.set(key, [item])
        } else {
          collection.push(item)
        }
      })
      return Array.from(map)
    },

    async getWalletInfo() {
      const relayerApiUrl = 'http://localhost:8081/v2'
      this.relayerClient = new HttpClient(relayerApiUrl)

      const injectedProviderIfExists = window.web3.currentProvider
      const networkId = await new Web3Wrapper(
        injectedProviderIfExists
      ).getNetworkIdAsync()

      /*const signerProvider =
        injectedProviderIfExists.isMetaMask || injectedProviderIfExists.isToshi
          ? new MetamaskSubprovider(injectedProviderIfExists)
          : new SignerSubprovider(injectedProviderIfExists)
      const provider = new Web3ProviderEngine()
      provider.addProvider(signerProvider)
      provider.addProvider(new RPCSubprovider('https://rinkeby.infura.io'))
      provider.start()
      */

      const signerProvider =
        injectedProviderIfExists.isMetaMask || injectedProviderIfExists.isToshi
          ? new MetamaskSubprovider(injectedProviderIfExists)
          : new SignerSubprovider(injectedProviderIfExists)
      const providerEngine = new Web3ProviderEngine()
      providerEngine.addProvider(signerProvider)
      providerEngine.addProvider(
        new RPCSubprovider('https://rinkeby.infura.io')
      )
      providerEngine.start()
      this.providerEngine = providerEngine
      this.web3Wrapper = new Web3Wrapper(providerEngine)
      this.contractWrappers = new ContractWrappers(providerEngine, {
        networkId
      })
      const addresses = await this.web3Wrapper.getAvailableAddressesAsync()
      console.log('addresses are: ' + addresses)
      this.maker = addresses[0]
      this.taker = addresses[1]
      console.log(
        'Owner address: ' + addresses[0] + ' at network: ' + networkId
      )
      const weiBalance = await this.web3Wrapper.getBalanceInWeiAsync(this.maker)
      this.ethBalance = weiBalance.toString()
      console.log('Eth balance: ' + weiBalance)

      this.walletNFT = await this.$axios.$get(
        'https://rinkeby-api.opensea.io/api/v1/assets?owner=' +
          this.maker +
          '&order_by=current_price&order_direction=asc'
      )

      //this.blocky = toDataUrl(this.maker)
      //const web3 = new Web3(
      //  new Web3.providers.HttpProvider('https://rinkeby.infura.io')
      //)

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

      //ifNetworkId != 1 Give error
      // Fetch all the Balances for all of the tokens
      // const allBalancesAsync = tokens.map(token => {
      /*
      try {
        const balance = await this.contractWrappers.erc721Token.getTokenCountAsync(
          //use the ERC721 tokenwrapper from 0x directly?
          '0x07f96aa816c1f244cbc6ef114bb2b023ba54a2eb', //'0x8f8221afbb33998d8584a2b05749ba73c37a938a', //0xf176d7bcdD07f8e474877095870685Ef0CCcCb2D
          this.maker
        )
        console.log('NFT token balance: ' + balance)
      } catch (e) {
        console.log(e)
      }*/
    },
    async mintToken() {
      // Mint a new ERC721 token for the maker
      const dummyERC721TokenContracts = []

      for (const tokenAddress of [
        '0xffce3807ac47060e900ce3fb9cdad3597c25425d'
      ]) {
        dummyERC721TokenContracts.push(
          new DummyERC721TokenContract(
            DummyERC721Token.compilerOutput.abi,
            tokenAddress,
            this.providerEngine
          )
        )
      }
      const mintTxHash = await dummyERC721TokenContracts[0].mint.sendTransactionAsync(
        this.maker,
        1,
        { from: this.maker }
      )
      await this.web3Wrapper.awaitTransactionSuccessAsync(mintTxHash)
    }
    /*,
    step(type) {
      var number = this.price
      if (number == 0) return 1

      var numberDup = number
      number = number.toString()
      console.log(this.price.length)
      console.log('numberString: ' + number.slice(0, -1))
      console.log(number[number.length - 1] + 1)
      return
      parseInt(numberDup) -
        (number.slice(0, -1) + number[number.length - 1] + 1)
    }
    */
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Nunito:300,400,600');

h1,
p,
.text-font {
  font-family: 'Nunito', Helvetica, sans-serif;
  text-align: center;
}

.subTitle {
  text-shadow: 0 5px 10px rgba(0, 0, 0, 0.33);
  position: relative;
}

.sell {
  background: #fff;
  border-radius: 6px;
  -webkit-box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  box-shadow: 0 20px 40px -15px rgba(0, 0, 0, 0.5);
  margin-bottom: 15px;
  overflow: hidden;
  margin-top: 0;
  max-width: 600px;
  margin: 0 auto;
  text-align: initial;
}
</style>
