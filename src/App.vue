<template>
  <div id="app">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-6 col-md-offset-3 col-xs-12">
          <div class="row status_icons">
            <h2 class="text-center">TwitchTv Channels</h2>
            <hr>
            <div class="col-md-4 col-xs-4 centered both">
              <span class="fa-stack fa-3x"  @click="show='all'">
                <i class="fa fa-television fa-2x fa-stack-2x"></i>
                <strong class="fa-stack-1x status-text">ALL</strong>
              </span>
            </div>
            <div class="col-md-4 col-xs-4 centered online">
              <span class="fa-stack fa-3x" @click="show='online'">
                <i class="fa fa-television fa-2x fa-stack-2x"> </i>
                <strong class="fa-stack-1x status-text">Online</strong>
              </span>
            </div>
            <div class="col-md-4 col-xs-4 centered offline">
              <span class="fa-stack fa-3x centered" @click="show='offline'">
                <i class="fa fa-television fa-2x fa-stack-2x"></i>
                <strong class="fa-stack-1x status-text">Offline</strong>
              </span>
            </div>
          </div>
          <hr />
        </div>
        <!-- stautus sections -->
        <div class="row">
          <div class="col-md-6 col-md-offset-3">
            <div class="row">
              <section id="streams" class="section">
                <ChannelItem v-for="(channel, index) in visibleChannels" :channel="channel" :key="index"></ChannelItem>
              </section>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6 col-md-offset-3">

        </div>

      </div>
      <!-- streams -->
    </div>
    <!-- container -->

  </div>
</template>

<script>

import ChannelItem from './components/ChannelItem'
import axios from 'axios';

export default {
  name: 'app',
  components: {
    'ChannelItem': ChannelItem
  },
  data() {
    return {
      channels: [],
      show: 'all'
    }
  },
  computed: {
    visibleChannels: function(){
      return this.channels.filter(channel =>{
          if(this.show ==='all')
            return channel;
          if(this.show === 'online' && channel.stream)
            return channel;
          if(this.show === 'offline' && !channel.stream)
            return channel;
          });
    }
  },
  created() {

    var streamNames = ["ESL_SC2", "OgamingSC2", "cretetion", "freecodecamp", "storbeck", "habathcx", "RobotCaleb", "noobs2ninjas"];
    streamNames.forEach(function(channelName) {
      axios.get(
        "https://api.twitch.tv/kraken/channels/" + channelName,
        { headers: { 'Client-ID': 'jdom04d5ik5g7ugrz6e41e2e7g89e3b' } }
      )
        .then(response => {
          axios.get(
            'https://api.twitch.tv/kraken/streams/' + response.data.name,
            { headers: { 'Client-ID': 'jdom04d5ik5g7ugrz6e41e2e7g89e3b' } }
          ).then(streamsResponse => {
             this.channels.push({ 
               data: response.data, 
               name: response.data.name, 
               stream: streamsResponse.data.stream  });
          }).catch(e => console.log(e))
        })
        .catch(e => {
          console.log(e);
        })
    }, this);
  }
}
</script>
<style lang='css'>
@import '../node_modules/bulma/css/bulma.css';
@import '../node_modules/bootstrap/dist/css/bootstrap.css';
@import '../node_modules/font-awesome/css/font-awesome.css';
</style>

<style>

  h2 {
    font-size: 2em;
    color: black;
    margin-bottom: 15px;
    margin-top: 15px;
    letter-spacing: 1px;
  }

  article a {
    color: inherit !important;
    text-decoration: none !important;
  }

  .status-text {
    font-size: .4em;
    margin-left: .2em;
  }

  .stream_img {
    border-radius: 50%;
    border: 1px solid black;
  }

  .centered {
    text-align: center;
  }

  .online {
    color: green;
  }

  .online :hover {
    color: green;
  }

  .offline {
    color: red;
  }

  .offline :hover {
    color: red;
  }

  .both {
    color: orange;
  }

  .both :hover {
    color: orange;
  }

</style>
