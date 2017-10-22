<template>
    <article class='media'>
      <figure class="media-left">
        <p class="image is-64x64">
          <img :src="icon" class="stream_img img-responsive">
        </p>
      </figure>
      <div class="media-content">
        <div class="content">
          <p>
            <strong><a :href="'https://www.twitch.tv/' + channel.name">{{channel.name}}</a></strong> 
            <small>{{channel.data.game}}</small>
            <br> {{channel.data.status}}
          </p>
        </div>
      </div>
      <div class="media-right">
        <i class="fa fa-circle"
          :class="streamingStatus" aria-hidden="true"></i>
      </div>
    </article>
</template>

<script>

export default {
  name: 'ChannelItem',
  props: ['channel'],
  computed: 
    {
      streamingStatus : function(){
          return this.channel.stream ? "online" : "offline"; 
      },
      icon : function(){
        return  this.channel.data && this.channel.data.logo 
        ? this.channel.data.logo
        : 'https://s5.postimg.org/tgck4v97r/404a_prod_Not_Found_img.jpg';
      }
  }
}
</script>

