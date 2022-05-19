<template>
  <card>
    <div slot="header">
      <h4 class="card-title">
        {{ config.selectedDevice.name }} - {{ config.variableFullName }}
         <img :src="img" >
      </h4>
    </div>
  </card>
</template>

<script>
    export default {
        name: 'espCam',
        props: ['config'],
        
        data() {
            return {
                img:"",
                wsHost : process.env.ws_host,
                wsPort : process.env.ws_port
            };
        },
        mounted(){
            const WS_URL = `ws:///${this.wsHost}:${this.wsPort}`;
            const ws = new WebSocket(WS_URL);
            let urlObject;
            ws.onopen = () => console.log(`Connected to ${WS_URL}`);
            ws.onmessage = message => {
                const arrayBuffer = message.data;
                if(urlObject){
                    URL.revokeObjectURL(urlObject);
                }
                this.img= URL.createObjectURL(new Blob([arrayBuffer]));
            }
        }
    };
</script>
<style></style>