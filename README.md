# CoreWriter SDK



![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

 

![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)

 

![Chain: HyperEVM](https://img.shields.io/badge/chain-HyperEVM-purple.svg)



> The first Python SDK for placing HyperCore L1 orders from HyperEVM.

Built by [@felixdoteth](https://x.com/felixdoteth)

---

## Why this exists

Hyperliquid has two layers:
- **HyperCore** — the L1 orderbook. $3B+ daily volume. Where all trading happens.
- **HyperEVM** — EVM-compatible smart contract layer.

The `0x3333...3333` precompile lets EVM code fire actions directly on HyperCore L1. Until now, nobody had wrapped this into a usable SDK.

**With CoreWriter you can build:**
- Liquidation bots that repay debt and place hedge orders atomically
- On-chain limit order protocols
- Automated delta-neutral strategies
- Smart contracts that interact with the orderbook

---

## Installation

```bash
pip install web3 eth-abi requests
from corewriter import CoreWriter

cw = CoreWriter(private_key="0x...")

# Read live oracle prices
print(cw.get_oracle_price(0))   # BTC:  $71,717
print(cw.get_oracle_price(1))   # ETH:  $2,088
print(cw.get_oracle_price(17))  # HYPE: $31.48

# Check perp account balance
bal = cw.get_perp_balance()
print(bal['account_value'])

# Short 0.01 BTC perp at market
tx, status = cw.place_perp_order(
    asset=0,
    is_buy=False,
    limit_price=72000,
    size=0.01,
    tif=3
)
CoreWriter(private_key, rpc?)
Initialize. Defaults to https://rpc.hyperliquid.xyz/evm.
place_perp_order(asset, is_buy, limit_price, size, reduce_only?, tif?, cloid?)
Place a perpetual order on HyperCore.
place_spot_order(asset, is_buy, limit_price, size, reduce_only?, tif?, cloid?)
Place a spot order on HyperCore.
tif: 1=ALO 2=GTC 3=IOC
cancel_all(asset?)
Cancel all open orders for an asset.
transfer_to_perp(amount_usd)
Move USDC from EVM wallet to HyperCore perp account.
transfer_to_evm(amount_usd)
Move USDC from HyperCore perp account to EVM wallet.
get_oracle_price(asset_index)
Read oracle price via precompile 0x807.
get_perp_balance()
Returns account_value, withdrawable, margin_used.
get_spot_balance(token_index)
Read spot token balance via precompile 0x801.
import requests, time
from corewriter import CoreWriter

cw = CoreWriter(private_key="0x...")

def real_gold():
    r = requests.get("https://forex-data-feed.swissquote.com/public-quotes/bboquotes/instrument/XAU/USD")
    sq = r.json()[0]["spreadProfilePrices"][0]
    return (sq["bid"] + sq["ask"]) / 2

def paxg_perp():
    r = requests.post("https://api.hyperliquid.xyz/info", json={"type": "metaAndAssetCtxs"})
    for m, c in zip(r.json()[0]["universe"], r.json()[1]):
        if m["name"] == "PAXG":
            return float(c.get("midPx") or 0)

while True:
    paxg = paxg_perp()
    gold = real_gold()
    spread = (paxg - gold) / gold * 100
    print(f"PAXG: ${paxg:,.2f} | Gold: ${gold:,.2f} | Spread: {spread:+.4f}%")
    if spread > 0.5:
        print("Opportunity - shorting PAXG perp")
        cw.place_perp_order(asset=187, is_buy=False, limit_price=paxg*0.999, size=0.01, tif=3)
    time.sleep(5)
