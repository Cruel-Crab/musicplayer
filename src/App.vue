<template>
  <div id="app">
    <header>
      <h1 onclick="location.reload()">Marsiyah Player</h1>
    </header>
    <main>
      <div class="contain">
        <section class="player">
          <h2 id="Header">Now Playing : </h2>
          <marquee ><h2 class="song-title">{{ current.title }} : <span>{{ current.artist }}</span>
          </h2></marquee>
          <div class="controls">
            <button class="prev" @click="prev">Prev</button>
            <button class="play" v-if="!isPlaying" @click="play">Play</button>
            <button class="pause" v-else @click="pause">Pause</button>
            <button class="next" @click="next">Next</button>
          </div>
        </section>
        <section class="playlist">
          <h3>PlayList</h3>
          <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing': 'song'">
              {{ song.title  }} - {{ song.artist }}
          </button>
          </section>
      </div>
    </main>
    <footer id="Footer">
      <span>Made By &copy; 2020-Present || Mohammed Hirani</span>
    </footer>
  </div>
</template>

<script>
//import func from '../vue-temp/vue-editor-bridge';

export default {
  name: 'App',
  data (){
    return {
      current :{
        //title: "My Playlist"
      },
      index : 0,
      isPlaying : false,
      songs :[
        {
          title : 'Mushkil Se Dil Na Ghabra',
          artist : 'Shk Kausar Ali',
          src : require('./assets/Mushkil_se.mp3'),
        },
        {
          title : 'Karbala Un Vaya Laha karbala un',
          artist : 'Burhani Party',
          src : require('./assets/karbala un.mp3')
        },
        {
          title : 'Arsh Girne Laga',
          artist : 'Shk. Mushtaq',
          src : require('./assets/Arsh Girne Laga.mp3')
        },
        {
          title : 'Matam Karo Husain Alaihi salam no',
          artist : 'Jamea tus Saifiyah',
          src : require('./assets/Matam Karo.mp3')
        },
        {
          title : 'Zainab Ko Guma Kab Tha',
          artist : 'Jamea tus Saifiyah',
          src : require('./assets/zainab ko guma.mp3')
        },
        {
          title : 'Ya Husaina Par Hame Qurban Che',
          artist : 'Jamea tus Saifiyah',
          src : require('./assets/Ya Husaina Par.mp3')
        },

      ],
      player: new Audio()
    }
  },
  methods: {
    play (song){
      if (typeof song.src != "undefined"){
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      //When the song ends this block of code is to play the nexxt song on the queue
      this.player.addEventListener('ended', function() {
        this.index++;
        if( this.index  > this.songs.length -1){
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));

      this.isPlaying = true;
    },
    pause(){
      this.player.pause();
      this.isPlaying = false;
    },
    next(){
      this.index++;
      if( this.index  > this.songs.length -1){
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev(){
       this.index--;
      if( this.index  < 0){
        this.index =  this.songs.length -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }

  },
  created(){
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
  }
}
</script>

<style>
  * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
  }
  body{
    font-family:Arial, Helvetica, sans-serif;

  }
  header,footer{
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #314345;
    color: white;
    padding: 15px;
  }
  #Header{
    font-weight: 600;
    font-family: 'Times New Roman', Times, serif;
    text-align: center;
    padding: 15px;
    text-decoration: underline;
  }
  #Footer{
    font-family: 'Times New Roman', Times, serif;
    text-align: center;
    padding: 10px;
    text-decoration: underline;
  }
  main{

    width: 100%;
    max-width:1200px;
    margin: 0 auto;
    padding: 25px ;
    
    box-sizing: border-box;
    background-color:white ;
  }
  .contain{
    /* background-image: linear-gradient(to right,#f7b554, #f7ad40); */
    background-color: rgb(224, 231, 20);
    border: 1px solid black;
    border-radius: 20px;
    padding-bottom: 15px;
  }
  .song-title{
    color: #212121;
    font-size: 29px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
  }
  .song-title span{
    font-weight: 400;
    font-style: italic;

  } 
  .controls{
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
  }
  button{
    appearance: none;
    background:none;
    border:none;
    outline:none;
    cursor: pointer;
  }
  button:hover{
    opacity: 0.8;
  }
  .play, .pause{
      font-size: 20px;
      font-weight: 700;
      padding: 15px 25px;
      margin: 0px 15px;
      color: #fff;
      border-radius: 8px;
      background: #CC2E5D;
  }
  .prev ,.next{
    font-size: 17px;
      font-weight: 700;
      padding: 10px 20px;
      margin: 0px 15px;
      color: #fff;
      border-radius: 6px;
      background: #ff5858;
  }
  .playlist{
    padding: 0px 30px;
  }
  .playlist h3{
    color: #212121;
    font-size: 26px;
    font-weight: 450;
    margin-bottom:28px ;
    text-align: center;
  }
  .playlist .song {
    display: block;
    width: 100%;
    padding: 14px;
    font-size: 19px;
    font-weight: 600;
    cursor: pointer;
  }
  .playlist .song:hover{
    color: #fff
  }
  .playlist .song.playing{
    color: #fff;
    background-image: linear-gradient(to right, #cc3e5d,#ff5858);
  }
  body::-webkit-scrollbar {
  width: 15px;               /* width of the entire scrollbar */
}
  body::-webkit-scrollbar-track {
  background: #f1f1f1;        /* color of the tracking area */
}
  body::-webkit-scrollbar-thumb {
  background-color: #888;    /*color of the scroll thumb */
  border-radius: 20px;       /* roundness of the scroll thumb */
  border: 3px solid orange;  /* creates padding around scroll thumb */
}
  body::-webkit-scrollbar-thumb:hover {
  
  background: #314345; 
} 
</style>
