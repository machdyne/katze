# Katze Ethernet Pmod&trade; compatible module

Katze is a work-in-progress 10/100 Ethernet PHY PMOD designed to be compatible with LiteX.

![Katze Ethernet Pmod Compatible Module](https://github.com/machdyne/katze/blob/12e5f9e5acae7f7d649d55cec78fd22e0d13b3a0/katze.png)

This repo contains schematics, pinouts and usage examples.

Find more information on the [Katze product page](https://machdyne.com/product/katze-ethernet-pmod/).

## Usage

At the moment it is necessary to use [our fork of LiteEth](https://github.com/machdyne/liteeth) which adds support for setting the configuration straps during reset. There are more details in this [pull request](https://github.com/enjoy-digital/liteeth/pull/126).

### Example Litex Platform Configuration

```
	("eth_clocks", 0,
		Subsignal("ref_clk", Pins("PMOD:7")),
		IOStandard("LVCMOS33")
	),
	("eth", 0,
		Subsignal("rx_data", Pins("PMOD:4 PMOD:5")),
		Subsignal("tx_data", Pins("PMOD:0 PMOD:1")),
		Subsignal("tx_en", Pins("PMOD:2")),
		Subsignal("crs_dv", Pins("PMOD:3")),
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
		hw_init_mode_cfg=[1,1,1],
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
