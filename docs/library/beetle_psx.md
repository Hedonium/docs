# PlayStation (Beetle PSX)

**Page last revised on: {{ git_revision_date }}**

## Background

Beetle PSX is a port/fork of Mednafen's PSX module to the libretro API. It can be compiled in C++98 mode. Beetle PSX currently runs on Linux, OSX and Windows.

Notable additions in this fork are:

- PBP and CHD file format support, developed by Zapeth;
- Software renderer internal resolution upscaling, implemented by simias;

The Beetle PSX core has been authored by

- [Mednafen Team](https://mednafen.github.io/)

The Beetle PSX core is licensed under

- [GPLv2](https://github.com/libretro/beetle-psx-libretro/blob/master/COPYING)

A summary of the licenses behind RetroArch and its cores have found [here](https://docs.libretro.com/tech/licenses/).

## BIOS

Required or optional firmware files go in the frontend's system directory.

|   Filename   | Description                         |              md5sum              |
|:------------:|:-----------------------------------:|:--------------------------------:|
| scph5500.bin | PS1 JP BIOS - Required for JP games | 8dd7d5296a650fac7319bce665a6a53c |
| scph5501.bin | PS1 US BIOS - Required for US games | 490f666e1afb15b7362b406ed1cea246 |
| scph5502.bin | PS1 EU BIOS - Required for EU games | 32736f17079d0b2b7024407c39bd3050 |

## Extensions

Content that can be loaded by the Beetle PSX core have the following file extensions:

- .cue
- .toc
- .m3u
- .ccd
- .exe
- .pbp
- .chd

RetroArch database(s) that are associated with the Beetle PSX core:

- [Sony - PlayStation](https://github.com/libretro/libretro-database/blob/master/rdb/Sony%20-%20PlayStation.rdb)

## Features

Frontend-level settings or features that the Beetle PSX core respects.

| Feature           | Supported |
|-------------------|:---------:|
| Restart           | ✔         |
| Screenshots       | ✔         |
| Saves             | ✔         |
| States            | ✔         |
| Rewind            | ✔         |
| Netplay           | ✕         |
| Core Options      | ✔         |
| RetroAchievements | ✕         |
| RetroArch Cheats  | ✔         |
| Native Cheats     | ✕         |
| Controls          | ✔         |
| Remapping         | ✔         |
| Multi-Mouse       | ✔         |
| Rumble            | ✔         |
| Sensors           | ✕         |
| Camera            | ✕         |
| Location          | ✕         |
| Subsystem         | ✕         |
| [Softpatching](https://docs.libretro.com/guides/softpatching/) | ✕         |
| Disk Control      | ✔         |
| Username          | ✕         |
| Language          | ✕         |
| Crop Overscan     | ✕         |
| LEDs              | ✕         |

### Directories

The Beetle PSX core's library name is 'Beetle PSX'

The Beetle PSX core saves/loads to/from these directories.

**Frontend's Save directory**

- Memory cards

**Frontend's State directory**

| File     | Description |
|:--------:|:-----------:|
| *.state# | State       |

### Geometry and timing

- The Beetle PSX core's core provided FPS is 59.941 for NTSC games and 49.76 for PAL games
- The Beetle PSX core's core provided sample rate is 44100 Hz
- The Beetle PSX core's base width is 320
- The Beetle PSX core's base height is 240
- The Beetle PSX core's max width is 700 when the 'Internal GPU resolution' is set to 1x. Raising the resolution past 1x will increase the max width
- The Beetle PSX core's max height is 576 when the 'Internal GPU resolution' is set to 1x. Raising the resolution past 1x will increase the max height
- The Beetle PSX core's core provided aspect ratio is 4/3 when the 'Widescreen mode hack' core option is set to off. 16/9 when it's set to on

## Loading content

Beetle PSX needs a cue-sheet that points to an image file. A cue sheet, or cue file, is a metadata file which describes how the tracks of a CD or DVD are laid out.

If you have e.g. `foo.bin`, you should create a text file and save it as `foo.cue`. Most PS1 games are single-track, so the cue file contents should look like this:

`foobin.cue`
```
 FILE "foo.bin" BINARY
  TRACK 01 MODE1/2352
   INDEX 01 00:00:00
```

After that, you can load the `foo.cue` file in RetroArch with the Beetle PSX core.

!!! attention
    Certain PS1 games are multi-track, so their .cue files might be more complicated.
	
### Playing PAL copy protected games

PAL copy protected games need a SBI Subchannel file next to the bin/cue files in order to get past the copy protection.

- Ape Escape (Europe).bin
- Ape Escape (Europe).cue
- **Ape Escape (Europe).sbi**

!!! warning
	For proper PAL game compatibility, the 'Skip BIOS' core option needs to be set to off.

### Multiple-disk games

If foo is a multiple-disk game, you should have .cue files for each one, e.g. `foo (Disc 1).cue`, `foo (Disc 2).cue`, `foo (Disc 3).cue`.

To take advantage of Beetle PSW's Disk Control feature for disk swapping, an index file (a m3u file) should be made.

Create a text file and save it as `foo.m3u`. Then enter your game's .cue files on it. The m3u file contents should look something like this:

`foo.m3u`
```
foo (Disc 1).cue
foo (Disc 2).cue
foo (Disc 3).cue
```

After that, you can load the `foo.m3u` file in RetroArch with the Beetle PSX core.

Here's a m3u example done with Valkryie Profile

![](/image/core/beetle_psx_hw/m3u.png)

!!! attention
	Adding multi-track games to a RetroArch playlist is recommended. (Manually add an entry a playlist that points to `foo.m3u`)

#### Swapping disks	

Swapping disks follows this procedure

1. Open tray (Disk Cycle Tray Status)

2. Change the Disk Index to the disk you want to swap to. 

3. Close tray (Disk Cycle Tray Status)

4. Return to the game and wait a few seconds to let it take effect
	
### Compressed content

Alternatively to using cue sheets with .bin/.iso files, you can convert your games to .pbp (Playstation Portable update file) or .chd (MAME Compressed Hunks of Data) to reduce file sizes and neaten up your game folder.

#### PBP

A recommended .pbp convert tool is PSX2PSP.

If converting a multiple-disk game, all disks should be added to the same .pbp file, rather than making a .m3u file for them.

Most conversion tools will want a single .bin file for each disk. If your game uses multiple .bin files (tracks) per disk, you will have to mount the cue sheet to a virtual drive and re-burn the images onto a single track before conversion.

For multi-disk PAL copy-proected games, change the sbi file syntax from [filename].sbi to [filename]_[disc_number].sbi

- Final Fantasy IX (Germany).pbp
- **Final Fantasy IX (Germany)_1.sbi**
- **Final Fantasy IX (Germany)_2.sbi**
- **Final Fantasy IX (Germany)_3.sbi**
- **Final Fantasy IX (Germany)_4.sbi**

!!! attention
    RetroArch does not currently have .pbp database due to variability in users' conversion methods. All .pbp games will have to be added to playlists manually.

#### CHD	

To convert content to CHD format, use the chdman tool found inside the latest MAME distribution and point it to a .cue file, like so:

```
chdman createcd --input foo.cue --output foo.chd
```

Note that the tool currrently does not integrate .sbi files into the .chd, so these must be placed alongside the resulting .chd file in order to properly play games with LibCrypt protection.

!!! attention
	For multi-disc content, make an .m3u file that lists all the .chd files instead of .cue files. Like the PBP files, content must be added to playlists manually.

## Saves

For game savedata storage, the PSX console used memory cards. The PSX console had two slots for memory cards. 

In this doc, the first memory card slot will be referred to as 'Memcard slot 0' and the second slot will be referred to as 'Memcard slot 1'.

For memory card functionality and usage, the Beetle PSX core will either use the Libretro savedata format or the Mednafen savedata format.

<center>

| Libretro savedata format   |  Mednafen savedata format        |
|----------------------------|----------------------------------|
| gamename.srm               |   gamename.slot#.mcr             |

</center>

**By default**, the Beetle PSX core will use Libretro's savedata format for Memcard slot 0 and Mednafen's savedata format for Memcard slot 1.

<center>

| Memcard slot 0 | Memcard slot 1 |
|----------------|----------------|
| gamename.srm   | gamename.1.mcr |

</center>

!!! attention
    Memory card behavior can be controlled with the following [core options](https://docs.libretro.com/library/beetle_psx/#core-options) (Memcard 0 method, Enable memory card 1, Shared memcards).
	
**By default**, the filenames of the Memcard savedata will match the loaded cue or m3u or pbp filename, like this:

- Loaded content: Breath of Fire III (USA).cue

- **Memcard slot 0: Breath of Fire III (USA).srm**

- **Memcard slot 1: Breath of Fire III (USA).1.mcr**

or

- Loaded content: Final Fantasy VII (USA).m3u

- **Memcard slot 0: Final Fantasy VII (USA).srm**

- **Memcard slot 1: Final Fantasy VII (USA).1.mcr**

or

- Loaded content: Wild Arms 2 (USA).pbp

- **Memcard slot 0: `Wild Arms 2 (USA).srm**

- **Memcard slot 1: `Wild Arms 2 (USA).1.mcr**

!!! attention
	To import your old memory cards from other emulators, you need to rename them to either the Libretro savedata format or the Mednafen savedata format.
	
!!! warning
	Keep in mind that save states also include the state of the memory card; carelessly loading an old save state will **OVEWRITE** the memory card, potentially resulting in lost saved games.	**You can set the 'Don't overwrite SaveRAM on loading savestate' option in RetroArch's Saving settings to On to prevent this.**

## Core options

The Beetle PSX core has the following option(s) that can be tweaked from the core options menu. The default setting is bolded. 

Settings with (Restart) means that core has to be closed for the new setting to be applied on next launch.

- **Internal GPU resolution** [beetle_psx_internal_resolution] (**1x(native)**/2x/4x/8x/16x/32x)

	Modify the resolution.
	
??? note "*Internal GPU Resolution - 1x*"
    ![](/image/core/beetle_psx_hw/gpu_1.png)

??? note "*Internal GPU Resolution - 2x*"
    ![](/image/core/beetle_psx_hw/gpu_2.png)

- **Widescreen mode hack** [beetle_psx_widescreen_hack] (**Off**/On)

	If on, renders in 16:9. Works best on 3D games.
	
??? note "Widescreen mode hack - Off"
	![](/image/core/beetle_psx_hw/wide_off.png)
	
??? note "Widescreen mode hack - On"
	![](/image/core/beetle_psx_hw/wide_on.png)
	
- **Frame duping (speedup)** [beetle_psx_frame_duping_enable] (**Off**/On)

	Redraws/reuses the last frame if there was no new data.
	
- **CPU frequency scaling (overclock)** [beetle_psx_cpu_freq_scale] (50% to 500% in increments of 10%. **100% is default**)
	
	Overclock the emulated PSX's CPU.
	
- **GTE Overclock** [beetle_psx_gte_overclock] (**Off**/On)

	Gets rid of memory access latency and makes all GTE instructions have 1 cycle latency.
	
- **GPU rasterizer overclock** [beetle_psx_gpu_overclock] (**1x(native)**/2x/4x/8x/16x/32x)

	Overclock the emulated PSX's GPU rasterizer.
	
- **Skip BIOS** [beetle_psx_skipbios] (**Off**/On)

	Self-explanatory. 
	
	**Some games have issues when this core option is enabled (Saga Frontier, PAL copy protected games, etc).**
	
??? note "Skip BIOS - Off"
	![](/image/core/beetle_psx_hw/bios.png)
	
- **Dithering pattern** [beetle_psx_dither_mode] (**1x(native)**/internal resolution/Off)

	If off, disables the dithering pattern the PSX applies to combat color banding.
	
- **Display internal FPS** [beetle_psx_display_internal_framerate] (**Off**/On)

	Shows the frame rate at which the emulated PSX is drawing at. 
	
	**Onscreen Notifications must be enabled in the RetroArch Onscreen Display Settings.**
	
??? note "Display internal FPS - On"
	![](/image/core/beetle_psx_hw/fps.png)
	
- **Initial scanline** [beetle_psx_initial_scanline] (0 to 40 in increments of 1. **0 is default**)

	Sets the first scanline to be drawn on screen.
	
- **Last scanline** [beetle_psx_last_scanline] (210 to 239 in increments of 1. **239 is default**)

	Sets the last scanline to be drawn on screen.
	
- **Initial scanline PAL** [beetle_psx_initial_scanline_pal] (0 to 40 in increments of 1. **0 is default**)

	Sets the first scanline to be drawn on screen for PAL systems.
	
- **Last scanline PAL** [beetle_psx_last_scanline_pal] (260 to 287 in increments of 1. **287 is default**)

	Sets the last scanline to be drawn on screen for PAL systems.
	
- **Crop Overscan** [beetle_psx_crop_overscan] (Off/**On**)

	Crop out the potentially random glitchy video output that would have been hidden by the bezel around the edge of a standard-definition television screen.
	
??? note "Crop Overscan - On"
	![](/image/core/beetle_psx_hw/scan_on.png)
	
??? note "Crop Overscan - Off"
	![](/image/core/beetle_psx_hw/scan_off.png)	

- **Additional Cropping** [beetle_psx_image_crop] (**Off**/1 px/2 px/3 px/4 px/5 px/6 px/7 px/8 px)

	Self-explanatory.
	
- **Offset Cropped Image** [beetle_psx_image_offset] (**Off**/1 px/2 px/3 px/4 px/-4 px/-3 px/-2 px/-1 px)

	Self-explanatory.
	
- **Analog self-calibration** [beetle_psx_analog_calibration] (**Off**/On)

	When enabled, monitors the max values reached by the input, using it as a calibration heuristic which then scales the analog coordinates sent to the emulator accordingly. For best results, rotate the sticks at max amplitude for the algorithm to get a good estimate of the scaling factor, otherwise it will adjust while playing.
	
- **DualShock Analog button toggle** [beetle_psx_analog_toggle] (**Off**/On)

	Toggles the Analog button from DualShock controllers, if disabled analogs are always on, if enabled you can toggle their state by pressing and holding START+SELECT+L1+L2+R1+R2.
	
- **Port 1: Multitap enable** [beetle_psx_enable_multitap_port1] (**Off**/On)

	Enables/Disables multitap functionality on port 1.
	
- **Port 2: Multitap enable** [beetle_psx_enable_multitap_port2] (**Off**/On)

	Enables/Disables multitap functionality on port 2.
	
- **Gun Cursor** [beetle_psx_gun_cursor] (**Cross**/Dot/Off)

	Choose the cursor for the 'Guncon / G-Con 45' and 'Justifier' Device Types. Setting it to off disables the crosshair.
	
??? note "Gun Cursor - Cross"
	![](/image/core/beetle_psx_hw/cursor_cross.png)
	
??? note "Gun Cursor - Dot"
	![](/image/core/beetle_psx_hw/cursor_dot.png)	

??? note "Gun Cursor - Off"
	![](/image/core/beetle_psx_hw/cursor_off.png)	
	
- **Mouse Sensitivity** [beetle_psx_mouse_sensitivity] (5% to 200% in increments of 5%. **100% is default**)

	Configure the 'Mouse' Device Type's sensitivity.
	
- **NegCon Twist Deadzone (percent)** [beetle_psx_negcon_deadzone] (**0**|5|10|15|20|25|30)

	Sets the deadzone of the RetroPad left analog stick when simulating the 'twist' action of emulated [neGcon Controllers](https://en.wikipedia.org/wiki/NeGcon). Used to eliminate drift/unwanted input.

!!! attention
	Most (all?) negCon compatible titles provide in-game options for setting a 'twist' deadzone value. To avoid loss of precision, the in-game deadzone should *always* be set to zero. Any analog stick drift should instead be accounted for by configuring the 'NegCon Twist Deadzone' core option. This is particularly important when 'NegCon Twist Response' is set to 'quadratic' or 'cubic'.
	
	Xbox gamepads typically require a deadzone of 15-20%. Many Android-compatible bluetooth gamepads have an internal 'hardware' deadzone, allowing the deadzone value here to be set to 0%.
	
	For convenience, it is recommended to make use of the 'Options → Analog Setting 1P' menu of [Gran Turismo](https://en.wikipedia.org/wiki/Gran_Turismo_(video_game)) when calibrating the 'NegCon Twist Deadzone'. This provides a clear and precise representation of 'real' controller input values.

- **NegCon Twist Response** [beetle_psx_negcon_response] (**linear**|quadratic|cubic)

	Specifies the analog response when using a RetroPad left analog stick to simulate the 'twist' action of emulated [neGcon Controllers](https://en.wikipedia.org/wiki/NeGcon).
	
	'linear': Analog stick displacement is mapped linearly to negCon rotation angle.
	
	'quadratic': Analog stick displacement is mapped quadratically to negCon rotation angle. This allows for greater precision when making small movements with the analog stick.
	
	'cubic': Analog stick displacement is mapped cubically to negCon rotation angle. This allows for even greater precision when making small movements with the analog stick, but 'exaggerates' larger movements.
	
!!! attention
	A linear response is not recommended when using standard gamepad devices. The negCon 'twist' mechanism is substantially different from conventional analog sticks; linear mapping over-amplifies small displacements of the stick, impairing fine control. A linear response is only appropriate when using racing wheel peripherals.
	
	In most cases, the 'quadratic' option should be selected. This provides effective compensation for the physical differences between real/emulated hardware, enabling smooth/precise analog input.
	
- **CD Access Method (restart)** [beetle_psx_cd_access_method] (**sync**/async/precache)

	The precache setting loads the complete image in memory at startup. Can potentially decrease loading times at the cost of increased startup time.
	
- **Memcard 0 method** [beetle_psx_use_mednafen_memcard0_method] (**libretro**/mednafen)

	Choose the savedata format used for Memcard slot 0 (libretro or mednafen) . Look above at the [Saves section](https://docs.libretro.com/library/beetle_psx/#saves) for an explanation regarding the libretro and mednafen formats.
	
- **Enable memory card 1** [beetle_psx_enable_memcard1] (Off/**On**)

	Enable or disables Memcard slot 1. When disabled, games cannot save/load to Memcard slot 1. 
	
	**Memcard 1 must be enabled for game 'Codename Tenka'.**
	
- **Shared memcards (restart)** [beetle_psx_shared_memory_cards] (**Setting1**/Setting2)

	Games will share and save/load to the same memory cards.
	
	**The 'Memcard 0 method' core option needs to be set to 'mednafen' for the 'Shared memcards' core option to function properly.**

<center>

| Memcard slot 0                     | Memcard slot 1                     |
|------------------------------------|------------------------------------|
| mednafen_psx_libretro_shared.0.mcr | mednafen_psx_libretro_shared.1.mcr |

</center>

- **Increase CD loading speed** [beetle_psx_cd_fastload] (**2x (native)**/4x/6x/8x/10x/12x/14x)

	Can greatly reduce the loading times in games.

	**May not work correctly in all games. Some games may break if you set them past a certain speed.**

## User 1 - 8 device types

The Beetle PSX core supports the following device type(s) in the controls menu, bolded device types are the default for the specified user(s):

- None - Input disabled.
- [**PlayStation Controller**](https://en.wikipedia.org/wiki/PlayStation_Controller) - Joypad - PlayStation Controller (SCPH-1080)
- [DualShock](https://en.wikipedia.org/wiki/DualShock) - Joypad - DualShock (SCPH-1200)
- [Analog Controller](https://en.wikipedia.org/wiki/Dual_Analog_Controller) - Joypad - PlayStation Dual Analog Controller(SCPH-1180)
- [Analog Joystick](https://en.wikipedia.org/wiki/PlayStation_Analog_Joystick) - Joypad - PlayStation Analog Joystick (SCPH-1110)
- [Guncon / G-Con 45](https://en.wikipedia.org/wiki/GunCon) - Lightgun - Namco Gun Controller (SLEH-00007)
- [Justifier](https://en.wikipedia.org/wiki/Konami_Justifier) - Lightgun -  Konami Justifier lightgun peripheral (SLEH-00005, SLUH-00017)
- [Mouse](https://en.wikipedia.org/wiki/PlayStation_Mouse) - Mouse - PlayStation Mouse (SCPH-1090, SCPH-1030)
- [neGcon](https://en.wikipedia.org/wiki/NeGcon) - Joypad - Namco third party controller

## Rumble support

Rumble only works in the Beetle PSX core when

- The content being ran has rumble support.
- The frontend being used has rumble support.
- The joypad device being used has rumble support.
- The corresponding user's device type is set to **DualShock**

## Multitap support

Activating multitap support in compatible games can be configured by the ['Port 1: Multitap enable' and 'Port 2: Multitap enable' core options](https://docs.libretro.com/library/beetle_psx#core-options).

## Joypad

![](/image/controller/psx.png)

| User 1 - 8 input descriptors  | RetroPad Inputs                              | PlayStation Controller Inputs                  | DualShock Inputs                                | Analog Controller Inputs                        | Analog Joystick Inputs                         | neGcon Inputs                   |
|-------------------------------|----------------------------------------------|------------------------------------------------|-------------------------------------------------|-------------------------------------------------|------------------------------------------------|---------------------------------|
| Cross                         | ![](/image/retropad/retro_b.png)             | ![](/image/Button_Pack/PS3/PS3_Cross.png)      | ![](/image/Button_Pack/PS3/PS3_Cross.png)       | ![](/image/Button_Pack/PS3/PS3_Cross.png)       | ![](/image/Button_Pack/PS3/PS3_Cross.png)      | Analog button I                 |
| Square                        | ![](/image/retropad/retro_y.png)             | ![](/image/Button_Pack/PS3/PS3_Square.png)     | ![](/image/Button_Pack/PS3/PS3_Square.png)      | ![](/image/Button_Pack/PS3/PS3_Square.png)      | ![](/image/Button_Pack/PS3/PS3_Square.png)     | Analog button II                |
| Select                        | ![](/image/retropad/retro_select.png)        | ![](/image/Button_Pack/PS3/PS3_Select.png)     | ![](/image/Button_Pack/PS3/PS3_Select.png)      | ![](/image/Button_Pack/PS3/PS3_Select.png)      | ![](/image/Button_Pack/PS3/PS3_Select.png)     |                                 |
| Start                         | ![](/image/retropad/retro_start.png)         | ![](/image/Button_Pack/PS3/PS3_Start.png)      | ![](/image/Button_Pack/PS3/PS3_Start.png)       | ![](/image/Button_Pack/PS3/PS3_Start.png)       | ![](/image/Button_Pack/PS3/PS3_Start.png)      | Start                           |
| D-Pad Up                      | ![](/image/retropad/retro_dpad_up.png)       | ![](/image/Button_Pack/PS3/PS3_Dpad_Up.png)    | ![](/image/Button_Pack/PS3/PS3_Dpad_Up.png)     | ![](/image/Button_Pack/PS3/PS3_Dpad_Up.png)     | ![](/image/Button_Pack/PS3/PS3_Dpad_Up.png)    | D-Pad Up                        |
| D-Pad Down                    | ![](/image/retropad/retro_dpad_down.png)     | ![](/image/Button_Pack/PS3/PS3_Dpad_Down.png)  | ![](/image/Button_Pack/PS3/PS3_Dpad_Down.png)   | ![](/image/Button_Pack/PS3/PS3_Dpad_Down.png)   | ![](/image/Button_Pack/PS3/PS3_Dpad_Down.png)  | D-Pad Down                      |
| D-Pad Left                    | ![](/image/retropad/retro_dpad_left.png)     | ![](/image/Button_Pack/PS3/PS3_Dpad_Left.png)  | ![](/image/Button_Pack/PS3/PS3_Dpad_Left.png)   | ![](/image/Button_Pack/PS3/PS3_Dpad_Left.png)   | ![](/image/Button_Pack/PS3/PS3_Dpad_Left.png)  | D-Pad Left                      |
| D-Pad Right                   | ![](/image/retropad/retro_dpad_right.png)    | ![](/image/Button_Pack/PS3/PS3_Dpad_Right.png) | ![](/image/Button_Pack/PS3/PS3_Dpad_Right.png)  | ![](/image/Button_Pack/PS3/PS3_Dpad_Right.png)  | ![](/image/Button_Pack/PS3/PS3_Dpad_Right.png) | D-Pad Right                     |
| Circle                        | ![](/image/retropad/Retro_a.png)             | ![](/image/Button_Pack/PS3/PS3_Circle.png)     | ![](/image/Button_Pack/PS3/PS3_Circle.png)      | ![](/image/Button_Pack/PS3/PS3_Circle.png)      | ![](/image/Button_Pack/PS3/PS3_Circle.png)     | A                               |
| Triangle                      | ![](/image/retropad/Retro_x.png)             | ![](/image/Button_Pack/PS3/PS3_Triangle.png)   | ![](/image/Button_Pack/PS3/PS3_Triangle.png)    | ![](/image/Button_Pack/PS3/PS3_Triangle.png)    | ![](/image/Button_Pack/PS3/PS3_Triangle.png)   | B                               |
| L1                            | ![](/image/retropad/Retro_l1.png)            | ![](/image/Button_Pack/PS3/PS3_L1.png)         | ![](/image/Button_Pack/PS3/PS3_L1.png)          | ![](/image/Button_Pack/PS3/PS3_L1.png)          | ![](/image/Button_Pack/PS3/PS3_L1.png)         | Left shoulder button (analog)   |
| R1                            | ![](/image/retropad/Retro_r1.png)            | ![](/image/Button_Pack/PS3/PS3_R1.png)         | ![](/image/Button_Pack/PS3/PS3_R1.png)          | ![](/image/Button_Pack/PS3/PS3_R1.png)          | ![](/image/Button_Pack/PS3/PS3_R1.png)         | Right shoulder button (digital) |
| L2                            | ![](/image/retropad/Retro_l2.png)            | ![](/image/Button_Pack/PS3/PS3_L2.png)         | ![](/image/Button_Pack/PS3/PS3_L2.png)          | ![](/image/Button_Pack/PS3/PS3_L2.png)          | ![](/image/Button_Pack/PS3/PS3_L2.png)         | Analog button II                |
| R2                            | ![](/image/retropad/Retro_r2.png)            | ![](/image/Button_Pack/PS3/PS3_R2.png)         | ![](/image/Button_Pack/PS3/PS3_R2.png)          | ![](/image/Button_Pack/PS3/PS3_R2.png)          | ![](/image/Button_Pack/PS3/PS3_R2.png)         | Analog button I                 |
| L3                            | ![](/image/retropad/Retro_l3.png)            |                                                  | ![](/image/Button_Pack/PS3/PS3_L3.png)          |                                                   |                                                |                                 |
| R3                            | ![](/image/retropad/Retro_r3.png)            |                                                  | ![](/image/Button_Pack/PS3/PS3_R3.png)          |                                                   |                                                |                                 |
| Left Analog X                 | ![](/image/retropad/Retro_left_stick.png) X  |                                                  | ![](/image/Button_Pack/PS3/PS3_Left_Stick.png)  | ![](/image/Button_Pack/PS3/PS3_Left_Stick.png)  | Left Joystick X                                | Twist                           |
| Left Analog Y                 | ![](/image/retropad/Retro_left_stick.png) Y  |                                                  | ![](/image/Button_Pack/PS3/PS3_Left_Stick.png)  | ![](/image/Button_Pack/PS3/PS3_Left_Stick.png)  | Left Joystick Y                                |                                 |
| Right Analog X                | ![](/image/retropad/Retro_right_stick.png) X |                                                  | ![](/image/Button_Pack/PS3/PS3_Right_Stick.png) | ![](/image/Button_Pack/PS3/PS3_Right_Stick.png) | Right Joystick X                               |                                 |
| Right Analog Y                | ![](/image/retropad/Retro_right_stick.png) Y |                                                  | ![](/image/Button_Pack/PS3/PS3_Right_Stick.png) | ![](/image/Button_Pack/PS3/PS3_Right_Stick.png) | Right Joystick Y                               |                                 |

## Mouse

| RetroMouse Inputs                                   | Mouse Inputs       |
|-----------------------------------------------------|--------------------|
| ![](/image/retromouse/retro_mouse.png) Mouse Cursor | Mouse Cursor       |
| ![](/image/retromouse/retro_left.png) Mouse 1       | Mouse Left Button  |
| ![](/image/retromouse/retro_right.png) Mouse 2      | Mouse Right Button |

## Lightgun

| RetroLightgun Inputs                                 | Guncon / G-Con 45 Inputs    | Justifier Inputs    |
|------------------------------------------------------|-----------------------------|---------------------|
| ![](/image/retromouse/retro_mouse.png) Gun Crosshair | Guncon / G-Con 45 Crosshair | Justifier Crosshair |
| Gun Trigger                                          | Guncon / G-Con 45 Trigger   | Justifier Trigger   |
| Gun Reload                                           | Guncon / G-Con 45 Reload    | Justifier Reload    |
| Gun Aux A                                            | Guncon / G-Con 45 A         | Justifier Aux       |
| Gun Aux B                                            | Guncon / G-Con 45 B         |                     |
| Gun Start                                            |                             | Justifier Start     |

## Compatibility

A list of known emulation bugs can be found here [https://forum.fobby.net/index.php?t=msg&th=1114&start=0&](https://forum.fobby.net/index.php?t=msg&th=1114&start=0&)

## External Links

- [Official Mednafen Website](https://mednafen.github.io/)
- [Official Mednafen Downloads](https://mednafen.github.io/releases/)
- [Beetle PSX Libretro Core info file](https://github.com/libretro/libretro-super/blob/master/dist/info/mednafen_psx_libretro.info)
- [Beetle PSX Libretro Github Repository](https://github.com/libretro/beetle-psx-libretro)
- [Report Beetle PSX Core Issues Here](https://github.com/libretro/beetle-psx-libretro/issues)

## PSX

- [PlayStation (Beetle PSX HW)](https://docs.libretro.com/library/beetle_psx_hw/)
- [PlayStation (PCSX ReARMed)](https://docs.libretro.com/library/pcsx_rearmed/)
