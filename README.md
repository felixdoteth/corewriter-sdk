# corewriter-sdk

The first Python SDK for HyperCore L1 on HyperEVM.

Built by [@felixdoteth](https://x.com/felixdoteth)

## What is CoreWriter?

Contract at 0x3333333333333333333333333333333333333333 on HyperEVM.
Trigger HyperCore L1 actions directly from EVM transactions.

## Installation

pip install web3 eth-abi requests

## Usage

from corewriter import CoreWriter
cw = CoreWriter(private_key="0x...")

cw.get_oracle_price(0)        # BTC price
cw.transfer_to_perp(100.0)    # EVM -> Perp
cw.transfer_to_evm(100.0)     # Perp -> EVM
cw.place_spot_order(10000, True, 70000, 0.001)  # spot order
cw.place_perp_order(0, True, 70000, 0.001)      # perp order
cw.cancel_all()               # cancel orders
cw.get_perp_balance()         # check balance

## Oracle Indices

0 = BTC, 1 = ETH, 17 = HYPE

## Notes

- Actions are NOT atomic with EVM tx
- Orders execute on next L1 block
- Chain ID: 999

## License

MIT
