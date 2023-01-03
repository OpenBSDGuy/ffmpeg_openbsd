# FFmpeg OpenBSD

FFmpeg scripts for screen recording YouTube videos and YouTube live streaming on OpenBSD.

# Stream Key

If you intend to do live streaming, make sure to place your Stream Key under the `~/Document/.streamkey` file.

# Enable microphone

The microphone is disabled by default on OpenBSD.

Temporary enable:

```bash
$ doas sysctl kern.audio.record=1
```

Permanently:

```bash
# echo kern.audio.record=1 >> /etc/sysctl.conf
```

# Audio playback

The scripts are capable of recording/streaming audio playback as well. It is done by 
utilzing `mon0.` in sndio. You need to enable that or modify the scripts.

Enabling `mon`:

```bash
$ doas rcctl set sndiod flags -v 80 -s default -m play,mon -s mon && doas rcctl restart sndiod
```

You can specify the volume of playback with `-v` flag.

# Microphone volume control

It is impossible to control microphone volume using FFmpeg. You need to set it with `sndio` or `cmixer`. I recommend the latter.

Install cmixer:

```bash
$ doas pkg_add cmixer
```

If you open the program, it defaults to `snd/0`, your internal sound card.

To switch the sound card, you must export the `AUDIODEVICE` variable and then open the mixer.

Example,

```bash
$ export AUDIODEVICE=snd/1 # pointing to the first external sound card
$ cmixer
$ unset AUDIODEVICE
```

Putting it all together, you can create an alias,

```bash
$ alias cmixer-microphone='export AUDIODEVICE=snd/1;cmixer;unset AUDIODEVICE'
```

# Source

- [OpenBSD FAQ 13 - Multimedia](https://www.openbsd.org/faq/faq13.html)
