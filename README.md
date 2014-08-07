upstream
========

[![Build Status](https://drone.io/github.com/angstwad/upstream/status.png)](https://drone.io/github.com/angstwad/upstream/latest)

Command line tool for uploading and downloading files from Metadisk.

## Chunk Class
The chunk class is for stores information about an encrypted chunk, including its hash and decryption key. This allows us to be able to covert between various formats needed in this tool and [Metadisk](https://github.com/storj/metadisk). 

### Example Usage 

	filehash = "5547a152337de9ff6a97f6f099bb024e08af419cee613b18da76a33e581d49ac"
	decryptkey = "2b77e64156f9f7eb16d74b98f70417e4d665d977d0ef00e793d41767acf13e8c"
	full_chunk = Chunk(filehash, decryptkey)
	print(full_chunk.get_json())

```json
{  
   "key":"2b77e64156f9f7eb16d74b98f70417e4d665d977d0ef00e793d41767acf13e8c",
   "filehash":"5547a152337de9ff6a97f6f099bb024e08af419cee613b18da76a33e581d49ac"
}
```

### Full Spec
* Chunk([filehash], [decryptkey], [filename], [filepath])
* Chunk.load_uri(raw)
* Chunk.load_json(raw)
* Chunk.get_uri()
* Chunk.get_tuple()
* Chunk.get_json()
* Chunk.set_filename(filename)
* Chunk.set_filepath(filepath)
* Chunk.get_filename()
* Chunk.get_filepath()

## Streamer Class
These are the transfer functions that are used to upload and download raw data to/from a node.

### Upload Usage

	Upstream.upload(path) 

### Upload Example
File is uploaded via a POST request to the [Metadisk](http://metadisk.org) node via its [web-core](https://github.com/Storj/web-core#api-documentation) API. There is currently a node running [here](http://node1.storj.io). Currently our default chunk size is 32 MB.

	up = Upstream("http://node1.storj.io")
	chunk = up.upload("C:\\Users\\super3\\Code\\upstream\\test.txt")

### Download Usage

	Upstream.upload(Chunk, destination) 

### Download Example
	
	up = Upstream("http://node1.storj.io")
	filehash = "5547a152337de9ff6a97f6f099bb024e08af419cee613b18da76a33e581d49ac"
	decryptkey = "2b77e64156f9f7eb16d74b98f70417e4d665d977d0ef00e793d41767acf13e8c"
	get_chunk = Chunk(filehash, decryptkey)
	up.download(get_chunk, "C:\\Users\\super3\\Code\\upstream\\files\\test.txt")

### Full Spec

* Streamer(server, [chunk_size])
* Streamer.get_server()
* Streamer.set_server()
* Streamer.get_chunk_size([units])
* Streamer.set_chunk_size(chunk_size, [units])
* Streamer().check_connectivity()
* Streamer().upload(path)
* Streamer().download(Chunk, [destination])

## Shredder Class
For shredding and merging larger files to be uploaded via Upstream. 

### Full Spec

* Shredder(filepath, chunk)
* Shredder.shred_chunks()
* Shredder.merge_chunks()
