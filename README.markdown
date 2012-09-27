# codec_silk

## What's this

This is a codec translator for [asterisk][asterisk] that translates signed linear to [Skype's SILK][silk].

## What this is not

This is not the official SILK translator provided by Digium. If you're running asterisk on Linux, you should get that one [from Digium][astsilk].

This is not a SILK encoder/decoder. The coder is provided by the [SILK library][silk], which you have to get separately, compile, and link against.

## System Requirements

This module builds against the Asterisk 10 source, so you need to be able to do that to use this module.

## Why

SILK support was added in Asterisk 10, but the translator module is provided as a binary download for Linux. I'm running asterisk on OS X, so can't use the binary module. Digium doesn't provide the source for their translator, but Skype provides the codec source. This module wraps the Skype SILK library into a translator for asterisk.

## How to use

0. Presuming that you already have asterisk built and installed somewhere, you can build and install this module like so:

1. create build directory and change to it

    `mkdir build`

    `cd build`

2. use cmake to create Makefile

    `cmake ..`

3. use make to download silk codec library and build codec_silk

    `make`

4. Install the new module. 

    `make install`

5. check if you have silk-codec enabled in codecs.conf, see [Asterisk 10 Codecs and Audio Formats][codec.conf]

    `asterisk -rvvv`

    `*CLI> module load codec_silk.so`

You should see all of the translator definitions get loaded along with their computational cost. If you don't, you might have to add the silk codec definitions to your codecs.conf, then restart asterisk.

## Compatability

I have tested this on OS X Server 10.6.

This module should support high sample rate codecs (16KHz, etc) without incident, but I have really only tested it with 8KHz codecs.

I have added support for the SILK native Packet Loss Concealment (PLC), but haven't tested it extensively. Support for the SILK redundant data decoding (looking in future frames for redundant data to decode in place of a lost frame) is not done though. Suggestions / pointers for working with the asterisk jitterbuffer are welcome.

## Feedback

Suggestions / bug reports / pull requests are welcome. This is my first time writing anything for asterisk and first repo on github, so please be kind.

[silk]: http://developer.skype.com/silk
[codec.conf]: https://wiki.asterisk.org/wiki/display/AST/Asterisk+10+Codecs+and+Audio+Formats
[asterisk]: http://www.asterisk.org/
[astsilk]: http://downloads.digium.com/pub/telephony/codec_silk/

