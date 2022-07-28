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
</template>

<script>
import host from "@elliemae/em-ssf-guest";
export default {
  data() {
    return {
      originationContext: {},
      callStatus: "<span style='color:grey'>Not Triggered</span>",
    };
  },
  methods: {
    async initializeOriginationContext() {
      try {
        this.callStatus =
          "<span style='color:orange'>Triggered and waiting for the response</span>";
        const transactionObject = await host.getObject("transaction");
        const originationContext = transactionObject.getOrigin();
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
  },
  created() {
    this.initializeOriginationContext();
  },
};
</script>

<style scoped>
a {
  color: #42b983;
}
</style>
