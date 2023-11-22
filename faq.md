Q: How to publish a new post onto blockchain?
A: plugins/chain.js
    Ln 1: import { Asset, Client, PrivateKey } from '@hiveio/dhive'

    Ln 8:  const client = new Client(rpcNode, {                         failoverThreshold: 20, 
                                consoleOnFailover: true })

            const getClient = () => client

            const chain = {
                Asset,
                Client,
                PrivateKey,
                PublicKey,
                cryptoUtils,
                ...utils,
                client,
                getClient
            }

         ***   inject('chain', chain)

nuxt.config.js
        Ln 44   plugins: [
            '@/plugins/axios.js',
            '@/plugins/chain.js'

store/index.js
    Ln 251  const client = this.$chain.getClient()

    Ln 273 await client.broadcast.sendOperations(operations, privateKey)

store/post.js
    Ln 38       export const actions = {
                            requestBroadcastPost ({ state, dispatch }, payload) {
    Ln 112      const emitData = { author, permlink, *** body}
    Ln 114      dispatch('requestBroadcastOps', {           
                            operations, emitEvent, emitData }, 
                          { root: true })
                

pages/publish.vue

    Ln 584      this.requestBroadcastPost({
                        title,
                        permlink,
                        body,