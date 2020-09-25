<template>
  <div id="app">
    <header>
      <h1>Spotted Fly Records</h1>
    </header>
    <section class="playlist">
      <div class="albums">
        <div
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src === current.src) ? 'song-playing' : 'song'"
        >
          <img :src="song.album" />
          <strong>{{ song.title }}</strong>
          <span style="font-size: 12px;">{{ song.artist }}</span>
        </div>
      </div>
    </section>
    <section class="player">
      <h2 class="song-title">
        {{ current.title }} -
        <span>{{current.artist}}</span>
      </h2>
      <div class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
      <div class="progress-bar-track">
        <div class="progress-bar" :style="{width: songProgress}"></div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      current: {},
      index: 0,
      songs: [
        {
          title: "Juicy",
          artist: "Notorius B.I.G.",
          src: require("./assets/juicy-biggie.mp3"),
          album: require("./assets/ReadyToDie.jpg"),
        },
        {
          title: "Rhymin' & Stealin'",
          artist: "Beastie Boys",
          src: require("./assets/rhymin-stealin.mp3"),
          album: require("./assets/LicenseToIll.jpg"),
        },
        {
          title: "Face in the Clouds",
          artist: "Sandpeople",
          src: require("./assets/Face-in-the-Clouds.mp3"),
          album: require("./assets/Sandpeople.jpg"),
        },
      ],
      player: new Audio(),
      playerInterval: null,
      songProgress: "",
      isPlaying: false,
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
        this.index = this.songs.indexOf(song);
      }

      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  max-height: 100vh;
  max-width: 100vw;
  overflow: hidden;
  text-align: center;
}

::selection {
  background-color: #42b883;
  color: #212121;
}

#app {
  background: #212121;
  width: 100vw;
  height: 100vh;
  color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #42b883;
}

button {
  border: none;
  background: none;
  color: #fff;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  margin: 16px 8px;
  cursor: pointer;
}

button:hover {
  color: #42b883;
}

button:focus {
  outline: none;
}

.play,
.pause {
  border: 1px solid #fff;
}

.albums {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.song,
.song-playing {
  max-width: 200px;
  min-height: 250px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.2s all ease-in-out;
  cursor: pointer;
  margin: 8px;
  padding: 16px;
  background: #212121;
  border-radius: 4px;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.65);
}

.song-playing {
  min-width: 300px;
  height: auto;
}

.song:hover {
  transform: scale(1.025);
}

.song img,
.song-playing img {
  max-width: 100%;
}

.song strong,
.song-playing strong {
  margin-top: 8px;
}

.controls {
  margin-bottom: 50px;
}

.progress-bar-track {
  height: 15px;
  width: 100vw;
  background: rgb(53, 73, 94);
  background: linear-gradient(
    27deg,
    rgba(53, 73, 94, 1) 1%,
    rgba(64, 166, 125, 1) 33%,
    rgba(28, 148, 97, 1) 66%,
    rgba(66, 184, 131, 1) 100%
  );
  position: relative;
}

.progress-bar {
  height: 15px;
  position: absolute;
  background: rgb(53, 73, 94);
  background: linear-gradient(
    27deg,
    rgba(53, 73, 94, 1) 1%,
    rgba(64, 166, 125, 1) 33%,
    rgba(28, 148, 97, 1) 66%,
    rgba(66, 184, 131, 1) 100%
  );
}
</style>
