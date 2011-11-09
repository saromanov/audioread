audioread
=========

Decode audio files using whichever library is available. The library currently
supports:

* [Gstreamer][] via [gst-python][].
* [Core Audio][] on Mac OS X via [ctypes][]. (PyObjC not required.)
* [MAD][] via the [pymad][] bindings.
* [FFmpeg][] via its command-line interface.

[Gstreamer]: http://gstreamer.freedesktop.org/
[gst-python]: http://gstreamer.freedesktop.org/modules/gst-python.html
[Core Audio]: http://developer.apple.com/technologies/mac/audio-and-video.html
[ctypes]: http://docs.python.org/library/ctypes.html
[pymad]: http://spacepants.org/src/pymad/
[MAD]: http://www.underbit.com/products/mad/
[FFmpeg]: http://ffmpeg.org/

Use the library like so:

    with audioread.audio_open(filename) as f:
        print f.channels, f.samplerate, f.duration
        for buf in f:
            do_something(buf)

Buffers in the file can be accessed by iterating over the object returned from
`audio_open`. Each buffer is a `buffer` or `str` object containing raw 16-bit
little-endian integer PCM data. (Currently, these PCM format attributes are
not configurable, but this could be added to most of the backends.)

Additional values are available as fields on the audio file object:

* `channels` is the number of audio channels (an integer).
* `samplerate` is given in Hz (an integer).
* `duration` is the length of the audio in seconds (a float).

Future Work
-----------

The library currently only selects the backend based on which supporting
libraries are available. In the future, it should "fall back" to a different
library when one decoder fails -- for instance, when one library supports a
certain audio format but another does not.

* PyOgg?
* Other command-line tools?

Example
-------

The included `decode.py` script demonstrates using this package to convert
compressed audio files to WAV files.

Et Cetera
---------

`audioread` is by Adrian Sampson. An alternative to this module is
[decoder.py][].

[decoder.py]: http://www.brailleweb.com/cgi-bin/python.py
