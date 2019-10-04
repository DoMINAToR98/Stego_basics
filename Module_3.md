# Steganography Toolkit 
Many different Linux and Windows tools are installed.
Windows tools are supported with [Wine](https://www.winehq.org/).
Some tools can be used on the command line while others require GUI support!
### Command line interface tools

These tools can be used on the command line.
All you have to do is start a container and mount the steganography files you want to check.

#### General screening tools

Tools to run in the beginning.
Allow you to get a broad idea of what you are dealing with.

|Tool          |Description       |How to use     |
|--------------|------------------|---------------|
| file         | Check out what kind of file you have | `file stego.jpg` |
| exiftool     | Check out metadata of media files | `exiftool stego.jpg` |
| binwalk      | Check out if other files are embedded/appended | `binwalk stego.jpg`
| strings      | Check out if there are interesting readable characters in the file | `strings stego.jpg`
| foremost     | Carve out embedded/appended files | `foremost stego.jpg`
| pngcheck     | Get details on a PNG file (or find out is is actually something else) | `pngcheck stego.png`
| identify     | [GraphicMagick](http://www.graphicsmagick.org/) tool to check what kind of image a file is. Checks also if image is corrupted. | `identify -verbose stego.jpg`
| ffmpeg       | ffmpeg can be used to check integrity of audio files and let it report infos and errors | `ffmpeg -v info -i stego.mp3 -f null -` to recode the file and throw away the result

#### Tools detecting steganography

Tools designed to detect steganography in files.
Mostly perform statistical tests.
They will reveal hidden messages only in simple cases.
However, they may provide hints what to look for if they find interesting irregularities.

|Tool          |File types                |Description       |How to use     |
|--------------|--------------------------|------------------|---------------|
| [stegoVeritas](https://github.com/bannsec/stegoVeritas) | Images (JPG, PNG, GIF, TIFF, BMP) | A wide variety of simple and advanced checks. Check out `stegoveritas.py -h`. Checks metadata, creates many transformed images and saves them to a directory, Brute forces LSB, ... | `stegoveritas.py stego.jpg` to run all checks |
| [zsteg](https://github.com/zed-0xff/zsteg) | Images (PNG, BMP) | Detects various LSB stego, also openstego and the [Camouflage tool](http://camouflage.unfiction.com/) | `zsteg -a stego.jpg` to run all checks |
| [stegdetect](http://old-releases.ubuntu.com/ubuntu/pool/universe/s/stegdetect) | Images (JPG) | Performs statistical tests to find if a stego tool was used (jsteg, outguess, jphide, ...). Check out `man stegdetect` for details. | `stegdetect stego.jpg` |
| [stegbreak](https://linux.die.net/man/1/stegbreak) | Images (JPG) | Brute force cracker for JPG images. Claims it can crack `outguess`, `jphide` and `jsteg`. | `stegbreak -t o -f wordlist.txt stego.jpg`, use `-t o` for outguess, `-t p` for jphide or `-t j` for jsteg |

#### Tools actually doing steganography

Tools you can use to hide messages and reveal them afterwards.
Some encrypt the messages before hiding them.
If they do, they require a password.
If you have a hint what kind of tool was used or what password might be right, try these tools.
Some tools are supported by the brute force scripts available in this Docker image.

|Tool          |File types                |Description       |How to hide    | How to recover |
|--------------|--------------------------|------------------|---------------|----------------|
| [AudioStego](https://github.com/danielcardeenas/AudioStego) | Audio (MP3 / WAV) | Details on how it works are in this [blog post](https://danielcardeenas.github.io/audiostego.html) | `hideme cover.mp3 secret.txt && mv ./output.mp3 stego.mp3` | `hideme stego.mp3 -f && cat output.txt` |
| [jphide/jpseek](http://linux01.gwdg.de/~alatham/stego.html) | Image (JPG) | Pretty old tool from [here](http://linux01.gwdg.de/~alatham/stego.html). Here, the version from [here](https://github.com/mmayfield1/SSAK) is installed since the original one crashed all the time. It prompts for a passphrase interactively! | `jphide cover.jpg stego.jpg secret.txt` | `jpseek stego.jpg output.txt` |
| [jsteg](https://github.com/lukechampine/jsteg) | Image (JPG) | LSB stego tool. Does not encrypt the message. | `jsteg hide cover.jpg secret.txt stego.jpg` | `jsteg reveal cover.jpg output.txt` |
| [mp3stego](http://www.petitcolas.net/steganography/mp3stego/) | Audio (MP3) | Old program. Encrypts and then hides a message (3DES encryption!). Windows tool running in Wine. Requires WAV input (may throw errors for certain WAV files. what works for me is e.g.: `ffmpeg -i audio.mp3 -flags bitexact audio.wav`). Important: use absolute path only! | `mp3stego-encode -E secret.txt -P password /path/to/cover.wav /path/to/stego.mp3` | `mp3stego-decode -X -P password /path/to/stego.mp3 /path/to/out.pcm /path/to/out.txt`
| [openstego](https://github.com/syvaidya/openstego) | Images (PNG) | Various LSB stego algorithms (check out this [blog](http://syvaidya.blogspot.de/)). Still maintained. | `openstego embed -mf secret.txt -cf cover.png -p password -sf stego.png` | `openstego extract -sf openstego.png -p abcd -xf output.txt ` (leave out -xf to create file with original name!) |
| [outguess](https://packages.debian.org/sid/utils/outguess) | Images (JPG) | Uses "redundant bits" to hide data. Comes in two versions: old=`outguess-0.13` taken from [here](https://github.com/mmayfield1/SSAK) and new=`outguess` from the package repos. To recover, you must use the one used for hiding. | `outguess -k password -d secret.txt cover.jpg stego.jpg` | `outguess  -r -k password stego.jpg output.txt` |
| [spectrology](https://github.com/solusipse/spectrology) | Audio (WAV) | Encodes an image in the spectrogram of an audio file. | `TODO` | Use GUI tool `sonic-visualiser` |
| [stegano](https://github.com/cedricbonhomme/Stegano) | Images (PNG) | Hides data with various (LSB-based) methods. Provides also some screening tools. | `stegano-lsb hide --input cover.jpg -f secret.txt -e UTF-8 --output stego.png` or `stegano-red hide --input cover.png -m "secret msg" --output stego.png` or `stegano-lsb-set hide --input cover.png -f secret.txt -e UTF-8 -g $GENERATOR --output stego.png` for various generators (`stegano-lsb-set list-generators`) | `stegano-lsb reveal -i stego.png -e UTF-8 -o output.txt` or `stegano-red reveal -i stego.png` or `stegano-lsb-set reveal -i stego.png -e UTF-8 -g $GENERATOR -o output.txt`
| [Steghide](http://steghide.sourceforge.net/) | Images (JPG, BMP) and Audio (WAV, AU) | Versatile and mature tool to encrypt and hide data. | `steghide embed -f -ef secret.txt -cf cover.jpg -p password -sf stego.jpg` | `steghide extract -sf stego.jpg -p password -xf output.txt`
| [cloackedpixel](https://github.com/livz/cloacked-pixel) | Images (PNG) | LSB stego tool for images | `cloackedpixel hide cover.jpg secret.txt password` creates `cover.jpg-stego.png` | `cloackedpixel extract cover.jpg-stego.png output.txt password`
| [LSBSteg](https://github.com/RobinDavid/LSB-Steganography) | Images (PNG, BMP, ...) in uncompressed formats | Simple LSB tools with very nice and readable Python code | `LSBSteg encode -i cover.png -o stego.png -f secret.txt` | `LSBSteg decode -i stego.png -o output.txt` |
| [f5](https://github.com/jackfengji/f5-steganography) | Images (JPG) | F5 Steganographic Algorithm with detailed info on the process | `f5 -t e -i cover.jpg -o stego.jpg -d 'secret message'` | `f5 -t x -i stego.jpg 1> output.txt` |
| [stegpy](https://github.com/dhsdshdhk/stegpy) | Images (PNG, GIF, BMP, WebP) and Audio (WAV) | Simple steganography program based on the LSB method | `stegpy secret.jpg cover.png` | `stegpy _cover.png` |
### Steganography GUI tools

All tools below have graphical user interfaces and cannot be used through the command line.
To run them, you must make an X11 server available inside the container.
Two ways are supported:
- run `start_ssh.sh` to fire up an SSH server. Connect afterwards with X11 forwarding. Requires an X11 server on your host!
- run `start_vnc.sh` to fire up a VNC server + client. Connect afterwards with your browser to port 6901 and you get an Xfce desktop. No host dependencies!

Alternatively, find other ways to make X11 available inside the container.
Many different ways are possible (e.g., [mount UNIX sockets](https://medium.com/@pigiuz/hw-accelerated-gui-apps-on-docker-7fd424fe813e)).

|Tool          |File types                |Description       |How to start    |
|--------------|--------------------------|------------------|----------------|
| [Steg](http://www.fabionet.org/) | Images (JPG, TIFF, PNG, BMP) | Handles many file types and implements different methods | `steg` |
| [Steganabara](http://www.caesum.com/handbook/stego.htm) (The [original link](http://www.freewebs.com/quangntenemy/steganabara/index.html) is broken) | Images (???) | Interactively transform images until you find something | `steganabara`|
| [Stegsolve](http://www.caesum.com/handbook/stego.htm) | Images (???) | Interactively transform images, view color schemes separately, ... | `stegsolve` |
| [SonicVisualiser](http://www.sonicvisualiser.org/) | Audio (???) | Visualizing audio files in waveform, display spectrograms, ... | `sonic-visualiser` |
| [Stegosuite](https://stegosuite.org/) | Images (JPG, GIF, BMP) | Can encrypt and hide data in images. Actively developed. | `stegosuite` |
| [OpenPuff](http://embeddedsw.net/OpenPuff_Steganography_Home.html) | Images, Audio, Video (many formats) | Sophisticated tool with long history. Still maintained. Windows tool running in wine. | `openpuff` |
| [DeepSound](http://jpinsoft.net/deepsound) | Audio (MP3, WAV) | Audio stego tool trusted by Mr. Robot himself. Windows tool running in wine (very hacky, requires VNC and runs in virtual desktop, MP3 broken due to missing DLL!) | `deepsound` only in VNC session |
| [cloackedpixel-analyse](https://github.com/livz/cloacked-pixel) | Images (PNG) | LSB stego visualization for PNGs - use it to detect suspiciously random LSB values in images (values close to 0.5 may indicate encrypted data is embedded) | `cloackedpixel-analyse image.png` |

Documentation by DominicBreuker
