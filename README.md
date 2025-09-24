# Spotify Clone (MVP)

A minimal Spotify clone built with Next.js and Tailwind CSS that lets you browse and play songs online.

## Features

- Browse a list of songs
- Play/pause/skip tracks
- Responsive layout

## Getting Started

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

---

Built for learning/demo purposes. For real music streaming, you must use licensed APIs.
{
  "name": "spotify-clone",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.3.0"
  }
}
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./pages/**/*.{js,jsx}",
    "./components/**/*.{js,jsx}"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-gray-900 text-white;
}
import '../styles/globals.css'

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
import Sidebar from '../components/Sidebar'
import SongList from '../components/SongList'
import AudioPlayer from '../components/AudioPlayer'
import songs from '../data/songs'
import { useState } from 'react'

export default function Home() {
  const [current, setCurrent] = useState(0)
  const [isPlaying, setIsPlaying] = useState(false)

  return (
    <div className="flex h-screen">
      <Sidebar />
      <main className="flex-1 flex flex-col">
        <div className="flex-1 p-8 overflow-auto">
          <h1 className="text-3xl font-bold mb-6">Spotify Clone</h1>
          <SongList
            songs={songs}
            current={current}
            setCurrent={setCurrent}
            setIsPlaying={setIsPlaying}
          />
        </div>
        <AudioPlayer
          song={songs[current]}
          isPlaying={isPlaying}
          setIsPlaying={setIsPlaying}
          onNext={() => setCurrent((current + 1) % songs.length)}
          onPrev={() => setCurrent((current - 1 + songs.length) % songs.length)}
        />
      </main>
    </div>
  )
}
export default function Sidebar() {
  return (
    <aside className="w-64 bg-gray-800 p-6 flex flex-col">
      <div className="text-2xl font-bold mb-8">Spotify Clone</div>
      <nav className="flex flex-col gap-4">
        <a className="hover:text-green-400" href="#">Home</a>
        <a className="hover:text-green-400" href="#">Search</a>
        <a className="hover:text-green-400" href="#">Your Library</a>
      </nav>
      <div className="mt-auto text-xs text-gray-400">Demo by Copilot</div>
    </aside>
  )
}
export default function SongList({ songs, current, setCurrent, setIsPlaying }) {
  return (
    <table className="w-full text-left">
      <thead>
        <tr>
          <th className="p-2">#</th>
          <th className="p-2">Title</th>
          <th className="p-2">Artist</th>
          <th className="p-2">Play</th>
        </tr>
      </thead>
      <tbody>
        {songs.map((song, i) => (
          <tr key={song.id} className={i === current ? "bg-gray-700" : ""}>
            <td className="p-2">{i + 1}</td>
            <td className="p-2">{song.title}</td>
            <td className="p-2">{song.artist}</td>
            <td className="p-2">
              <button
                onClick={() => {
                  setCurrent(i)
                  setIsPlaying(true)
                }}
                className="bg-green-500 hover:bg-green-600 text-white px-2 py-1 rounded"
              >
                Play
              </button>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  )
}
import { useRef, useEffect } from 'react'

export default function AudioPlayer({
  song,
  isPlaying,
  setIsPlaying,
  onNext,
  onPrev
}) {
  const audioRef = useRef(null)

  useEffect(() => {
    if (isPlaying) {
      audioRef.current.play()
    } else {
      audioRef.current.pause()
    }
  }, [isPlaying, song])

  return (
    <div className="bg-gray-800 p-4 flex items-center gap-6">
      <div>
        <div className="font-bold">{song.title}</div>
        <div className="text-sm text-gray-400">{song.artist}</div>
      </div>
      <audio
        ref={audioRef}
        src={song.url}
        onEnded={onNext}
        onPlay={() => setIsPlaying(true)}
        onPause={() => setIsPlaying(false)}
      />
      <div className="flex items-center gap-2 ml-4">
        <button onClick={onPrev} className="px-2 py-1 rounded bg-gray-600 hover:bg-gray-500">Prev</button>
        <button
          onClick={() => setIsPlaying((p) => !p)}
          className="px-4 py-1 rounded bg-green-500 hover:bg-green-600"
        >
          {isPlaying ? "Pause" : "Play"}
        </button>
        <button onClick={onNext} className="px-2 py-1 rounded bg-gray-600 hover:bg-gray-500">Next</button>
      </div>
    </div>
  )
}
// Some free preview tracks (royalty free or sample)
const songs = [
  {
    id: 1,
    title: "Acoustic Breeze",
    artist: "Bensound",
    url: "https://www.bensound.com/bensound-music/bensound-acousticbreeze.mp3"
  },
  {
    id: 2,
    title: "Going Higher",
    artist: "Bensound",
    url: "https://www.bensound.com/bensound-music/bensound-goinghigher.mp3"
  },
  {
    id: 3,
    title: "Sunny",
    artist: "Bensound",
    url: "https://www.bensound.com/bensound-music/bensound-sunny.mp3"
  }
]

export default songs
