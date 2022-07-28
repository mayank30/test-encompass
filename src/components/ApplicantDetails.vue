<template>
  <h1>originationContext:</h1>
  <pre>
    {{ originationContext }}
  </pre>

  <br />
  <br />
  <div>
    <h3>
      Status of <code>elli.script.getObject( "transaction" )</code> call:
      <span v-html="callStatus"></span>
    </h3>
    <div style="position: fixed; bottom: 10px; right: 10px">
      Last Updated: 13:31 28/07/2022 IST
    </div>
  </div>

  <br />

  <h3>To Initate Transaction Click here</h3>
  <button @click="createTransaction">Initate Transaction</button>
</template>

<script>
import host from "@elliemae/em-ssf-guest";
export default {
  data() {
    return {
      originationContext: {},
      transaction: null,
      callStatus: "<span style='color:grey'>Not Triggered</span>",
    };
  },
  methods: {
    async initializeOriginationContext() {
      try {
        this.callStatus =
          "<span style='color:orange'>Triggered and waiting for the response</span>";

        this.transaction = await host.getObject("transaction");
        const originationContext = this.transaction.getOrigin();
        console.log(originationContext);
        this.callStatus = "<span style='color:green'>Complete</span>";
        // applicationState.originId = originationContext.id;
        // applicationSate.partnerAccessToken =
        //   originationContext.partnerAccessToken;
        // if (originationContext.transactionId) {
        //   applicationState.transactionId = originationContext.transactionId;
        // }
      } catch (error) {
        console.log({ error });
      }
    },
    async createTransaction() {
      await this.transaction.create();
    },
  },
  async created() {
    host.connect();
    host.ready();
    await this.initializeOriginationContext();
  },
};
</script>

<style scoped>
a {
  color: #42b983;
}
</style>
