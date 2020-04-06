# StripLightLib

Datapath-based WS281x (Neopixel) driver for PSoC 3, 4 and 5.

This is a mod of the original StripLightLib project made by [MarkH_61](https://community.cypress.com/thread/16543) which primarily adds (currently limited) DMA support for PSoC 4.

## Motivation

The original interrupt-based implementation behaves strangely when there are other ISRs present. Since the transmitter (ironically) uses timing to determine whether or not a transfer is at its end, a hiccup caused by, e.g. critical section, that is long enough (1) is enough to starve the transmitter and cause premature termination of transfer. An example can be found [here](https://community.cypress.com/thread/34709).

There are several potential ways to fix/workaround this, including splitting the strip to different columns and transfer them column-by-column without column interrupts (which sounds a bit hacky to me), or tinkering with the interrupt priority to prioritize the row interrupt (which never worked for me), and so forth. However the ultimate solution IMO is moving to DMA. By using DMA the CPU involvement on row transfer became minimal (as in zero) and the timing issue mentioned above no longer existed.

(1): About 10us after some primitive inspection, which is even lower than the 50us reset/break mark of WS281x chips.

## Limitations

- The current DMA implementation only buffers a single column and relies on the column ISR to buffer the next, which could potentially increase interrupt latency by a lot. Future version may buffer the entire framebuffer at once instead.
- Significantly higher memory usage due to the raw 24-bit GRB buffer being always present no matter the use of CLUT or not. It's not as bad as the NeoPixel-ZeroDMA or NeoPXL8 but still quite significant compare to ISR mode.

## License

Ask Mark. Don't ask me. :S
