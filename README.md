# StripLightLib

Datapath-based WS281x (Neopixel) driver for PSoC 3, 4 and 5.

This is a mod of the original StripLightLib project made by [MarkH_61](https://community.cypress.com/thread/16543) which primarily adds (currently limited) DMA support for PSoC 4.

## Motivation

The original interrupt-based implementation behaves strangely when there are other ISRs present. Since the transmitter (ironically) uses timing to determine whether or not a transfer is at its end, a relatively long (1) hiccup caused by, e.g. critical section is enough to starve the transmitter and cause premature termination of transfer. An example can be found [here](https://community.cypress.com/thread/34709).

There are several potential ways to fix/workaround this, including splitting the strip to different columns and transfer them column-by-column without column interrupts (which sounds a bit hacky to me), or tinkering with the interrupt priority to prioritize the row interrupt (which never worked for me), and so forth. However the ultimate solution IMO is moving to DMA. By using DMA the CPU involvement on row transfer became minimal (as in practically zero) and the timing issue mentioned above no longer exist.

(1): About 10us after some preliminary inspections, which is only 1/5 of the 50us reset/break mark of WS281x chips.

## License

Ask Mark. Don't ask me. :S
