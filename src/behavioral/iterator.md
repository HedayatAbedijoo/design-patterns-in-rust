# Iterator Pattern

Provides a way to sequentially access elements of an aggregate object without exposing its underlying representation, allowing iteration over various data structures.

## Example

---

```rust
// Song struct
struct Song {
    title: String,
    artist: String,
    duration: u32,
}

// Music Collection
struct MusicCollection {
    songs: Vec<Song>,
}

impl MusicCollection {
    fn new() -> Self {
        MusicCollection { songs: Vec::new() }
    }

    fn add_song(&mut self, song: Song) {
        self.songs.push(song);
    }

    fn iter(&self) -> MusicIterator {
        MusicIterator::new(&self.songs)
    }
}

// Iterator for Music Collection
struct MusicIterator<'a> {
    songs: &'a Vec<Song>,
    current_index: usize,
}

impl<'a> MusicIterator<'a> {
    fn new(songs: &'a Vec<Song>) -> Self {
        MusicIterator {
            songs,
            current_index: 0,
        }
    }
}

impl<'a> Iterator for MusicIterator<'a> {
    type Item = &'a Song;

    fn next(&mut self) -> Option<Self::Item> {
        if self.current_index < self.songs.len() {
            let song = &self.songs[self.current_index];
            self.current_index += 1;
            Some(song)
        } else {
            None
        }
    }
}

fn main() {
    let mut music_collection = MusicCollection::new();

    // Add songs to the music collection
    music_collection.add_song(Song {
        title: "Song 1".to_string(),
        artist: "Artist 1".to_string(),
        duration: 180,
    });
    music_collection.add_song(Song {
        title: "Song 2".to_string(),
        artist: "Artist 2".to_string(),
        duration: 240,
    });
    music_collection.add_song(Song {
        title: "Song 3".to_string(),
        artist: "Artist 3".to_string(),
        duration: 210,
    });

    // Iterate over the songs in the music collection
    for song in music_collection.iter() {
        println!("Title: {}", song.title);
        println!("Artist: {}", song.artist);
        println!("Duration: {} seconds", song.duration);
        println!("-----------------------");
    }
}

```
