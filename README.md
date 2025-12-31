HDMI & SPDIF Bit-Stream Format Enabler for Windows 11

This repository provides a single Windows Registry (.reg) file that enables native HDMI and S/PDIF bit-stream output of modern multi-channel audio codecs on Windows 11. It:

    Removes any legacy or malformed codec entries

    Re-adds proper support for AC-3, DTS, E-AC-3 (Dolby Digital Plus), DTS-HD Master Audio and Dolby TrueHD

    Targets both 32-bit and 64-bit registry hives
    
Features

    Cleans out old or broken SPDIF_Formats entries in both the 32-bit and 64-bit registry views

    Recreates codec support entries under:

        HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\MMDevices\SPDIF_Formats

        HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\SPDIF_Formats

    Ensures HDMI endpoints that physically support bit-streaming will expose each format in Supported Formats

    Leaves Windows’ Protected Audio Path intact (no DRM hacks)

Supported Codecs

    Dolby Digital (AC-3)

    DTS Digital Surround

    Dolby Digital Plus (E-AC-3)

    DTS-HD Master Audio

    Dolby TrueHD

    Note: DTS:X is object-based and rides on top of DTS-HD or E-AC-3 containers. Enabling DTS-HD and E-AC-3 suffices for DTS:X pass-through.

How It Works

    Removal Deletes both 32-bit and 64-bit SPDIF_Formats keys to wipe out stale or incorrect entries.

    Recreation (32-bit) Writes new subkeys under the WOW6432Node hive for each supported codec, specifying a test-file resource in mmres.dll.

    Recreation (64-bit) Mirrors the same codec subkeys under the native MMDevices\SPDIF_Formats hive, ensuring 64-bit audio endpoints pick them up.

After import and a reboot (or a restart of the Windows Audio and Windows Audio Device Graph Isolation services), any HDMI or S/PDIF output device whose driver advertises bit-stream capability will list these codecs in its Supported Formats tab.
Requirements

Sound reg file: For openal + dsoundal
🎧 What IndirectSound Is

IndirectSound is a small compatibility layer that restores 3D positional audio (surround sound, hardware‑accelerated DirectSound3D effects) in older Windows games that lost this feature after Microsoft removed hardware audio acceleration starting with Windows Vista

What It Actually Does

IndirectSound works by:    Allowing those games to output true 3D positional surround sound again (rear, side, 5.1, 7.1 setups)

    Fixing games where 3D audio is missing or broken on modern Windows

    Acting as a drop‑in replacement for dsound.dll  in the game folder

    Restoring EAX‑style effects for some titles (via emulation)

It’s essentially a modern replacement for what Creative ALchemy does — but for any sound card, not just Sound Blaster hardware

    Emulating DirectSound3D hardware acceleration that old games expect

    
    Windows 11 (x64)
    

    Administrator privileges to import .reg files

    Audio driver with bit-stream support over SPDIF or HDMI

Installation

    Download the enable-bitstream-formats.reg file.

    Backup your registry or create a System Restore point.

    Right-click the .reg file → Merge → accept UAC prompt.

    Reboot your PC

    Detailed Behavior

All actions occur in these registry paths:

    32-bit hive HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\MMDevices\SPDIF_Formats\<Codec-GUID>

    64-bit hive HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\SPDIF_Formats\<Codec-GUID>

Each subkey defines:

    DisplayName – The name shown in the audio control panel

    TestFile – A hex-encoded pointer to a resource inside mmres.dll which verifies format support

By rebuilding these keys, Windows will again offer bit-streaming of AC-3, DTS, E-AC-3, DTS-HD MA and TrueHD on compatible hardware.
Troubleshooting

    If a format still doesn’t appear:

        Open Sound → Playback → select your SPDIF/HDMI device → Properties → Supported Formats.

        Check for any missing codecs.

        Locate your device’s GUID under: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\MMDevices\Audio\Render\<DeviceGUID>\…

        Verify driver version and update from your GPU or motherboard vendor if needed.

    To test actual bit-stream output, use a media player with built-in AC-3/DTS test clips or a Blu-ray test disc.
