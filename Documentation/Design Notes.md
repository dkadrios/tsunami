###Tapestry Design Notes:

The initial Tapestry audio engine board is designed to be used for development purposes, and provides the basic components
of a high performance audio DSP/Playback engine, including an Atmel SAM S70 Arm Cortex M7 CPU, a microSD socket connected
via SDIO, a high-speed USB port and an AKM multi-channel audio codec providing 4 channels of input and 6 channels of output.
It also includes 10-pin, 0.05" pitch SWD/JTAG connector for use with the Atmel SAM ICE debugger during firmware development.

The design assumes that most user interface controls and connnectors, audio buffering and connectors, as well as any specific
external power connection and regulation will be contained on a second application specific "UI" board. In this way, the audio
engine can be repurposed for different end-user applications and requirements.

**Power**

Vin is assumed to be 5V. The design intent is that 5V can be supplied either through the UI header or from the USB
connection, or both. If both are plugged in, the external 5V Vin will be favored. If a wider input range using a barrel or
other type of power connector is required, the connector itself and any additional regulation can located on the UI board.

**microSD Socket**

The microSD card interfaces to the CPU using the 4-bit wide SDIO interface rather than SPI. The Atmel SAM 70 CPU is able
to recognize high speed cards and run the SDIO clock at 50Mhz, making the layout of the card and routing of the SDIO traces
somewhat critical. SDIO signals and traces should be treated as high frequency.

**Codec**

The codec interfaces to the CPU using the SAM S70 SAI resource in TDM mode for audio, and I2C for control. The design allows
for up to 4 audio input channels and 6 audio output channels.  A dual-row, 20-pin connector JP4 provides the ac coupled audio
input and output connections to and from the codec. Any additional audio signal conditioning / buffering, as well as the
desired audio connectors are assumed to be on the UI board.

**User and Control Interface**

All user interface/control signals, including Vin, are via a dual-row 26 pin header JP3. There are 16 GPIO, 8 of which
can also be configured as analog inputs or outputs. The intent is that alternate firmware versions can use these lines to
interface to trigger or gate inputs, and/or user interface controls such as analog pots, encoders, LEDs, etc. There are also
2 UARTs (Tx and Rx), one of which will be dedicated for MIDI (with the MIDI opto-isolator, related components and connectors
being located on the UI board.)

**Test Points**

Test points are provided for the TDM signals between the SAM S70 and codec. Test points are also provide for a subset of
the SDIO signals to facilitate timing SD card access. A single general purpose test point is also provided for use by the
firmware.