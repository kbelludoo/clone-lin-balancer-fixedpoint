# clone-lin-balancer-fixedpoint

Experimental LIN clone of Balancer V2 `FixedPoint.mulDown` / `mulUp` / `divDown` / `divUp` / `complement` (i64-positive scale: 128-bit floor and ceil over `ONE=1e18`, fail-closed if the quotient does not fit i64).

- **Upstream:** [balancer/balancer-v2-monorepo](https://github.com/balancer/balancer-v2-monorepo) `pkg/solidity-utils/contracts/math/FixedPoint.sol` commit `e91a2b643a49856f51a648d175667c1b48cf3377` (GPL-3.0-or-later).
- **LIN kernel:** [`src/lin_balancer_fixedpoint.lin`](src/lin_balancer_fixedpoint.lin)
- **C11 oracle copy:** [`test/oracles/balancer_fixedpoint_c11.c`](test/oracles/balancer_fixedpoint_c11.c)
- **Proofs (lin-open only):** [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/balancer_fixedpoint/` and `python3 test/prove_balancer_fixedpoint_external.py`
- **Class:** EXPERIMENTAL. Not Ethereum Balancer V2 uint256. Not a production Vault/WeightedPool replacement. Not Aave half-up `wadMul`. Not `powDown`/`powUp`/`LogExpMath`.

Named goldens (i64-positive, ONE=1e18): `mulDown(3, 0.5e18)` → **1** vs `mulUp` **2** vs Aave half-up **2**; `divUp(1,3)` → **333333333333333334** vs Aave `wadDiv` **333333333333333333**; `mulUp(1,1)` → **1** vs `mulDown` **0**; `mulDown(3e18, 3e18)` → **9000000000000000000** vs naive i64 wrap **5595889181738926080**; `mulDown(4e18, 3e18)` fail-closed **0**.
