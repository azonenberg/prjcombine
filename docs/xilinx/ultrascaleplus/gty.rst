GTY Transceiver
############

Introduction
===============

GTY transceivers are organized in quads. Each quad contains one ``GTYE4_COMMON`` and four ``GTYE4_CHANNEL``. The primitives are very minimally documented; for most parameters the only mention in the datasheet is "use the magic value from the wizard". This page aims to provide some more fundamental understanding of the actual attributes, sufficient to enable using the raw primitives without the wizard.

GTYs can operate at a maximum data rate of 32.75 Gbps (package and speed grade dependent).

Clocking
-----------

TX and RX of each channel can be clocked independently from one of three sources:

* QPLL0 (in COMMON block, shared by all CHANNELs in the quad)
* QPLL1 (in COMMON block, shared by all CHANNELs in the quad)
* CPLL (in CHANNEL block, dedicated to this CHANNEL)

Each PLL has a different operating frequency range, which must be carefully considered when planning complex setups with multiple lanes in a quad running at different data rates.

In the case of KU+, the native data rates for each PLL at the nominal Vcore are as follows. To run at lower rates than shown here, the QPLL output divider and/or channel sub-rate modes must be used.

* CPLL: 4.0 to:
	* -1 speed: 8.5 Gbps
	* -2 or -3 speed: 12.5 Gbps
* QPLL0: 19.6 to:
    * -1 speed: 25.785 Gbps
    * -2 speed: 28.21 Gbps
    * -3 speed: 32.75 Gbps
* QPLL1: 16.0 to:
    * -1 speed: 25.785 Gbps
    * -2 or -3 speed: 26.0 Gbps

Check table 64 of DS922 for full details on voltage ranges and data rates.

Note that the internal transceiver drive circuitry is DDR, i.e. for 20 Gbps data rate the PLL VCO should be running at 10 GHz.

``GTYE4_COMMON``
==============

Attributes
-----------

* BIAS_CFG0
  Always 16'b0000000000000000
* BIAS_CFG1
  Always 16'b0000000000000000
* BIAS_CFG2
  Always 16'b0000010100100100
* BIAS_CFG3
  Always 16'b0000000001000001
* BIAS_CFG4
  Always 16'b0000000000010000
* BIAS_CFG_RSVD
  Always 16'b0000000000000000
* COMMON_CFG0
  Always 16'b0000000000000000
* COMMON_CFG1
  Always 16'b0000000000000000
* POR_CFG
  Always 16'b0000000000000000
* PPF0_CFG
  Something to do with QPLL0. Not yet fully understood. So far:
   * Bits 15:13: always 0
   * Bit 12: 1 if using fractional-N, 0 if not
   * Bit 11: 0 if using fractional-N, 1 if not
   * Bit 10: both 0 and 1 seen, but no clear pattern yet
   * Bits 9:0: always 0
* PPF1_CFG
  Seems to be same mapping as PPF0_CFG but for QPLL1
* QPLL0CLKOUT_RATE
  QPLL0 output divide-by-two control. Set to "HALF" to enable the divider or "FULL" to bypass it.
* QPLL0_CFG0
  Always 16'b0011001100011100
* QPLL0_CFG1
  Always 16'b1101000000111000
* QPLL0_CFG1_G3
  Always 16'b1101000000111000
* QPLL0_CFG2
  Something to do with QPLL0. Not yet fully understood. So far:
   * Bits 15:12: always 0
   * Bits 11:6: always 1
   * Bits 5:2: always 0
   * Bits 1:0: both 1 if using fractional-N, 0 if not
* QPLL0_CFG2_G3
  Always same as QPLL0_CFG2
* QPLL0_CFG3
  Always 16'b0000000100100000
* QPLL0_CFG4
  Something to do with QPLL0. Not yet fully understood. So far:
   * Bits 15:8: always 0
   * Bit 7: 1 if using fractional-N, 0 if not
   * Bits 6:3: always 0
   * Bit 2: 1 if using fractional-N, 0 if not
   * Bit 1: 0 if using fractional-N, 1 if not
   * Bit 0: both 0 and 1 seen, but no clear pattern yet
* QPLL0_CP
  Always 10'b0011111111
* QPLL0_CP_G3
  Always 10'b0000001111
* QPLL0_FBDIV
  QPLL0 feedback divider N. Set to an integer between 16 and 160 to control the PLL multiplier between VCO and PFD.
* QPLL0_FBDIV_G3
  Related to QPLL0 feedback divider but not yet understood. Values seen so far 160 and 128. Possible values in DRP range 16 to 160.
* QPLL0_INIT_CFG0
  Always 16'b0000001010110010
* QPLL0_INIT_CFG1
  Always 8'b00000000
* QPLL0_LOCK_CFG
  Always 16'b0010010111101000
* QPLL0_LOCK_CFG_G3
  Always 16'b0010010111101000
* QPLL0_LPF
   * Bit 9: always 1
   * Bits 8:6: always 0
   * Bit 5: 0 if using fractional-N, 1 if not
   * Bits 4:0: always 1
* QPLL0_LPF_G3
  Always 10'b0111010101
* QPLL0_PCI_EN
  Always 1'b0 in all configurations tested to date, but we have not tested anything using the PCIe IP.
* QPLL0_RATE_SW_USE_DRP
  Always 1'b1
* QPLL0_REFCLK_DIV
  QPLL0 reference clock divider. Set to an integer between 1 and 4 to control the input divider between refclk input and PFD.
  NOTE: according to UG578 table B-1, this attribute can also take the values 5, 6, 8, 10, 12, 16, and 20. Maybe the PLL doesn't like input frequencies this low?
* QPLL0_SDM_CFG0
   * Bits 15:8: always 0
   * Bit 7: 0 if using fractional-N, 1 if not
   * Bits 6:0: always 0
* QPLL0_SDM_CFG1
  So far, always 16'b0000000000000000
* QPLL0_SDM_CFG2
  So far, always 16'b0000000000000000
* QPLL1CLKOUT_RATE
  QPLL1 output divide-by-two control. Set to "HALF" to enable the divider or "FULL" to bypass it.
* QPLL1_CFG0
  TODO
* QPLL1_CFG1
  TODO
* QPLL1_CFG1_G3
  TODO
* QPLL1_CFG2
  TODO
* QPLL1_CFG2_G3
  TODO
* QPLL1_CFG3
  TODO
* QPLL1_CFG4
  * Bits 16:2: always 0
  * Bit 1: always 1
  * Bit 0: 0 for half rate mode, 1 for full rate mode (TODO verify with more configs)
* QPLL1_CP
  TODO
* QPLL1_CP_G3
  TODO
* QPLL1_FBDIV
  QPLL1 feedback divider N. Set to an integer between 16 and 160 to control the PLL multiplier between VCO and PFD.
* QPLL1_FBDIV_G3
  Related to QPLL1 feedback divider but not yet understood
* QPLL1_INIT_CFG0
  TODO
* QPLL1_INIT_CFG1
  TODO
* QPLL1_LOCK_CFG
  TODO
* QPLL1_LOCK_CFG_G3
  TODO
* QPLL1_LPF
  TODO
* QPLL1_LPF_G3
  TODO
* QPLL1_PCI_EN
  Always 1'b0 in all configurations tested to date, but we have not tested anything using the PCIe IP.
* QPLL1_RATE_SW_USE_DRP
  TODO
* QPLL1_REFCLK_DIV
  QPLL1 reference clock divider. Set to an integer between 1 and 4 to control the input divider between refclk input and PFD.
  NOTE: according to UG578 table B-1, this attribute can also take the values 5, 6, 8, 10, 12, 16, and 20. Maybe the PLL doesn't like input frequencies this low?
* QPLL1_SDM_CFG0
  TODO
* QPLL1_SDM_CFG1
  TODO
* QPLL1_SDM_CFG2
  TODO
* RSVD_ATTR0
  TODO
* RSVD_ATTR1
  TODO
* RSVD_ATTR2
  TODO
* RSVD_ATTR3
  TODO
* RXRECCLKOUT0_SEL
  TODO
* RXRECCLKOUT1_SEL
  TODO
* SARC_ENB
  TODO
* SARC_SEL
  TODO
* SDM0INITSEED0_0
  TODO
* SDM0INITSEED0_1
  TODO
* SDM1INITSEED0_0
  TODO
* SDM1INITSEED0_1
  TODO
* SIM_DEVICE
  Selects the simulation model to use, ignored for synthesis. Should always be set to "ULTRASCALE_PLUS"
* SIM_MODE
  Selects something related to simulation, ignored for synthesis. Should always be set to "FAST"
* SIM_RESET_SPEEDUP
  Selects a tradeoff between simulation fidelity and speed. Valid values:
      * "TRUE" (default) simplified reset model, fastest simulation
      * "FAST_ALIGN": speed up simulation of TX/RX buffer bypass mode
      * "FALSE": most accurate modeling of reset behavior
* UB_CFG0
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000
* UB_CFG1
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000
* UB_CFG2
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000
* UB_CFG3
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000
* UB_CFG4
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000
* UB_CFG5
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000010000000000
* UB_CFG6
  Unknown, related to the hard MicroBlaze in the COMMON. Should always be set to 16'b0000000000000000

Ports
-----------

``GTYE4_CHANNEL``
===============
