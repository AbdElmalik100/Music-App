<script setup>
import { ref, onMounted } from 'vue'

const current = ref({})
const index = ref(0)
const isPlaying = ref(false)
const player = new Audio()
const songs = ref([
  {
    title: "Burn it down",
    artist: "Linkin Park",
    src: './burn-it-down-linkin-park.mp3'
  },
  {
    title: "Da Da Da (Jarico Remix)",
    artist: "Tanir",
    src: './Да да да (Jarico Remix).mp3'
  },
])


function play(song) {
  if (typeof song.src != "undefined") {
    current.value = song
    player.src = current.value.src
    index.value = songs.value.indexOf(current.value)
  }
  player.play()
  isPlaying.value = true
}

function pause() {
  player.pause()
  isPlaying.value = false
}

function previous() {
  index.value++
  if (index.value > songs.value.length - 1) {
    index.value = 0
  }
  current.value = songs.value[index.value]
  play(current.value)
}

function next() {
  index.value--
  if (index.value < 0) {
    index.value = songs.value.length - 1
  }
  current.value = songs.value[index.value]
  play(current.value)
}


onMounted(() => {
  current.value = songs.value[index.value]
  player.src = current.value.src
})
</script>

<template>
  <header>
    <h1>My Music</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
      <div class="controls">
        <button class="prev" @click="previous">Prev</button>
        <button class="play" @click="play" v-if="!isPlaying">Play</button>
        <button class="pause" @click="pause" v-if="isPlaying">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="play-list">
      <h3>The Playlist</h3>
      <div class="list">
        <button v-for="song in songs" :key="song.src" @click="play(song)"
          :class="(song.src === current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </div>
    </section>
  </main>
</template>

<style scoped lang="scss">
* {
  box-sizing: border-box;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  background-color: #212121;
  color: white;
}

button {
  outline: none;
  border: none;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  opacity: 0.3;
}

main {
  text-align: center;
  margin-top: 50px;

  .player {
    .song-title {
      color: #53565A;
      text-transform: uppercase;
      font-weight: bold;
      font-size: 32px;

      span {
        font-weight: normal;
        font-style: italic;
      }
    }

    .controls {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 30px;
      margin: 25px 0;

      button {
        border-radius: 8px;
        padding: 15px 25px;
        font-weight: bold;
        color: white;
      }

      .play,
      .pause {
        background-color: #CC2E5D;
        font-size: 25px;
      }

      .next,
      .prev {
        background-color: #FF5858;
        font-size: 15px;
        padding: 10px 20px;
      }
    }
  }

  .play-list {
    padding: 0px 50px;

    h3 {
      color: #212121;
      font-size: 28px;
      margin-bottom: 25px;
    }

    .list {
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      gap: 25px;
    }

    button {
      outline: none;
      border: none;
      cursor: pointer;
      transition: 0.3s;
      padding: 15px 20px;
      width: 100%;
      font-weight: bold;
      color: #53565A;

      &.song:hover {
        color: #FF5858;
      }

      &.song.playing {
        color: white;
        background-image: linear-gradient(to right, #CC2E5D, #FF5858);
      }
    }
  }
}
</style>
