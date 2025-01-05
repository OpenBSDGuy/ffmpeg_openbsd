# Hardware acceleration for Intel GPUs (OpenBSD 7.6 and later)

If you have an Intel GPU, you can take advantage of the VA-API driver ported to OpenBSD 7.6, thanks to [Rafael Sadowski](https://rsadowski.de/). This reduces the load on the CPU during tasks like screencasting.

To get started, you first need to identify your Intel GPU generation to install the appropriate package.

## Identifying your GPU generation

### Core i 4th generation or older

If your CPU is from the Intel Core i 4th generation (Haswell) or older—for example, the Core i5-3320M (Ivy Bridge)—you have an Intel GPU from Gen7 or earlier. In this case, you need to install the [Intel VA_API Driver](https://github.com/intel/intel-vaapi-driver):

```bash
$ doas pkg_add intel-vaapi-driver libva-utils
```
 
### Core i 5th generation or newer

If your CPU is from the Intel Core i 5th generation (Broadwell) or newer—for example, the Core i5-8350U (Coffee Lake)—you have an Intel GPU from Gen8 or later. In this case, you should install the [Intel Media Driver](https://github.com/intel/media-driver):

```bash
$ doas pkg_add intel-media-driver libva-utils
```

**Note:** While both the Intel VA-API Driver and the Intel Media Driver technically support Gen8 and Gen9 GPUs, it is strongly recommended to use the Intel Media Driver for better performance and compatibility.


## Verifying hardware acceleration

After installing the appropriate driver, verify that hardware acceleration is working by running the following command:

```bash
$ vainfo
```

You should see output similar to this:

```
Trying display: x11
libva info: VA-API version 1.22.0
libva info: Trying to open /usr/X11R6/lib/modules/dri/iHD_drv_video.so
libva info: Trying to open /usr/local/lib/dri/iHD_drv_video.so
libva info: Found init function __vaDriverInit_1_22
libva info: va_openDriver() returns 0
vainfo: VA-API version: 1.22 (libva 2.22.0)
vainfo: Driver version: Intel iHD driver for Intel(R) Gen Graphics - 24.2.5 (OpenBSD)
vainfo: Supported profile and entrypoints
      VAProfileNone                   : VAEntrypointVideoProc
      VAProfileNone                   : VAEntrypointStats
      VAProfileMPEG2Simple            : VAEntrypointVLD
      VAProfileMPEG2Simple            : VAEntrypointEncSlice
      VAProfileMPEG2Main              : VAEntrypointVLD
      VAProfileMPEG2Main              : VAEntrypointEncSlice
      VAProfileH264Main               : VAEntrypointVLD
      VAProfileH264Main               : VAEntrypointEncSlice
      VAProfileH264Main               : VAEntrypointFEI
      VAProfileH264Main               : VAEntrypointEncSliceLP
      VAProfileH264High               : VAEntrypointVLD
      VAProfileH264High               : VAEntrypointEncSlice
      VAProfileH264High               : VAEntrypointFEI
      VAProfileH264High               : VAEntrypointEncSliceLP
      VAProfileVC1Simple              : VAEntrypointVLD
      VAProfileVC1Main                : VAEntrypointVLD
      VAProfileVC1Advanced            : VAEntrypointVLD
      VAProfileJPEGBaseline           : VAEntrypointVLD
      VAProfileJPEGBaseline           : VAEntrypointEncPicture
      VAProfileH264ConstrainedBaseline: VAEntrypointVLD
      VAProfileH264ConstrainedBaseline: VAEntrypointEncSlice
      VAProfileH264ConstrainedBaseline: VAEntrypointFEI
      VAProfileH264ConstrainedBaseline: VAEntrypointEncSliceLP
      VAProfileVP8Version0_3          : VAEntrypointVLD
      VAProfileVP8Version0_3          : VAEntrypointEncSlice
      VAProfileHEVCMain               : VAEntrypointVLD
      VAProfileHEVCMain               : VAEntrypointEncSlice
      VAProfileHEVCMain               : VAEntrypointFEI
      VAProfileHEVCMain10             : VAEntrypointVLD
      VAProfileHEVCMain10             : VAEntrypointEncSlice
      VAProfileVP9Profile0            : VAEntrypointVLD
      VAProfileVP9Profile2            : VAEntrypointVLD
```

## Checking VA-API encoders in FFmpeg

Ensure that FFmpeg has the appropriate VA-API encoders by running:

```bash
$ ffmpeg -hide_banner -encoders | grep vaapi
```

You should get output like this:

```
V....D h264_vaapi           H.264/AVC (VAAPI) (codec h264)
V....D hevc_vaapi           H.265/HEVC (VAAPI) (codec hevc)
V....D mjpeg_vaapi          MJPEG (VAAPI) (codec mjpeg)
V....D mpeg2_vaapi          MPEG-2 (VAAPI) (codec mpeg2video)
V....D vp8_vaapi            VP8 (VAAPI) (codec vp8)
V....D vp9_vaapi            VP9 (VAAPI) (codec vp9)
```

## Using hardware-accelerated scripts

Once everything is set up and confirmed, you can use scripts like `screencast_openbsd_hw` or `stream_openbsd_hd` to make the most of your hardware acceleration.
