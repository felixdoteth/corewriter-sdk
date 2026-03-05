# corewriter-sdk

The first Python SDK for interacting with HyperCore L1 directly from HyperEVM.

Built and discovered by [@felixdoteth](https://x.com/felixdoteth)

## What is CoreWriter?

CoreWriter is a contract at `0x3333333333333333333333333333333333333333` on HyperEVM that lets you trigger HyperCore L1 actions directly from EVM transactions.

## Installation

```bash
pip install web3 eth-abi requests
from corewriter import CoreWriter

cw = CoreWriter(private_key="0x...")

# Oracle prices
print(cw.get_oracle_price(0))   # BTC
print(cw.get_oracle_price(1))   # ETH
print(cw.get_oracle_price(17))  # HYPE

# Transfer $100 EVM -> Perp
tx, status = cw.transfer_to_perp(100.0)

# Place spot order
tx, status = cw.place_spot_order(
    asset=10000, is_buy=True, limit_price=70000, size=0.001
)

# Place perp order
tx, status = cw.place_perp_order(
    asset=0, is_buy=True, limit_price=70000, size=0.001
)

# Cancel all
tx, status = cw.cancel_all()

# Check balance
print(cw.get_perp_balance())
