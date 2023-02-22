# Katze Ethernet Pmod&trade; compatible module

Katze is a 10/100 Ethernet PHY PMOD designed to be compatible with LiteX.

![Katze Ethernet Pmod Compatible Module](https://github.com/machdyne/katze/blob/12e5f9e5acae7f7d649d55cec78fd22e0d13b3a0/katze.png)

This repo contains schematics, pinouts and usage examples.

Find more information on the [Katze product page](https://machdyne.com/product/katze-ethernet-pmod/).

## Usage

### Example Litex Platform Configuration

```
	("eth_clocks", 0,
		Subsignal("ref_clk", Pins("PMOD:7")),
		IOStandard("LVCMOS33")
	),
	("eth", 0,
		Subsignal("rx_data", Pins("PMOD:4 PMOD:5"), Misc("PULLMODE=UP")),
		Subsignal("tx_data", Pins("PMOD:0 PMOD:1")),
		Subsignal("tx_en", Pins("PMOD:2")),
		Subsignal("crs_dv", Pins("PMOD:3"), Misc("PULLMODE=UP")),
		Subsignal("rst_n", Pins("PMOD:6")),
		IOStandard("LVCMOS33")
	)
```

### Example LiteX Target Configuration

```
	from liteeth.phy.rmii import LiteEthPHYRMII
	self.submodules.ethphy = LiteEthPHYRMII(
		clock_pads = platform.request("eth_clocks"),
		pads = platform.request("eth"),
		with_hw_init_reset=True,
		refclk_cd=None)
	self.add_ethernet(phy=self.ethphy)
```

## Pinout

| Pin | Signal |
| --- | ------ |
| 1 | E\_TXD0 |
| 2 | E\_TXD1 |
| 3 | E\_TXEN |
| 4 | E\_CRS\_DV |
| 5 | GND |
| 6 | PWR3V3 |
| 7 | E\_RXD0 |
| 8 | E\_RXD1 |
| 9 | E\_RST\_N |
| 10 | CLK50 |
| 11 | GND |
| 12 | PWR3V3 |
