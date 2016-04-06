# StableGif

Uses [ffmpeg] with [vid.stab] to convert a series of photos from a handheld
camera into a gif video.


## Requirements

ffmpeg, compiled with vid.stab enabled.

Not sure if you have vid.stab enabled? Here's a quick check:
```sh
ffmpeg -v warning -filters | grep -q "vidstab" \
    && echo "vid.stab available" || echo "vid.stab unavailable"
```

If you don't have vid.stab enabled on your ffmpeg build (it is not enabled for
the build which ships with Ubuntu), you can either
[compile a fresh copy][ffmpeg-download] with `--enable-libvidstab` included in
the configuration,
or download a static build [for Linux][ffmpeg-linux-build] or
[other OSes][ffmpeg-download].
I recommend using a static build, as it is much easier and faster.


## Installation

These steps are intended for Linux-based systems.

You should also get good mileage on Mac OSX.

1.  Download or clone [this repository].

2.  Copy to stablegif your `~/bin` folder, and make it executable:
    ```sh
    cp stablegif ~/bin/stablegif && chmod +x ~/bin/stablegif
    ```

3.  If necessary, edit the file so the `ffmpeg_binary` variable is defined as
    the path to your ffmpeg binary with vid.stab available.
    By default, `ffmpeg` is used.


## Usage

Simply run
```sh
stablegif INPUTGLOB OUTPUTFILE
```
where `INPUTGLOB` is a glob matching the files to turn into a GIF
(such as `*.jpg`), and `OUTPUTFILE` is the path to the output file.

If only one input is given, the output file name will default to
```
<first-image-name-without-extension>-<last-image-name-without-extension>.gif
```

### Example

```sh
stablegif folder_containing_jpegs/*.jpg gif_folder/cool_clip.gif
```
or, with automatic output naming
```sh
stablegif folder_containing_jpegs/*.jpg
```


## Features

-   Settings are customisable at the start of the script.
-   A "[high-quality GIF]" is output, using a customised palette for this GIF
    based on the content in the photos.
    This is important because GIFs only contain 256 colours, and generating a
    custom palette will make the most of what is available.


## Notes

-   The input glob of files is assumed to list all the source images in the
    correct order.
    stablegif does not attempt to sort them itself.
    You can check what the order is with `ls -1 INPUTGLOB`.
-   As an intermediate step, the photos are converted into a lossy .mp4 file.
    This has negligable impact on the final output because the GIF is
    downscaled in width and height, and also in number of colours.
    Using a lossy intermediary is faster than saving and then reading a larger,
    uncompressed, intermediate file.


  [this repository]: https://github.com/scottclowe/subcaption2subfig
  [ffmpeg]: https://ffmpeg.org
  [ffmpeg-download]: https://ffmpeg.org/download.html
  [ffmpeg-linux-build]: http://johnvansickle.com/ffmpeg/
  [vid.stab]: https://github.com/georgmartius/vid.stab
  [high-quality GIF]: http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html
