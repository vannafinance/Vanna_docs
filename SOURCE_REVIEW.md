# Stellar testnet documentation refresh

Reviewed: 2026-09-12.

Updated `Vanna_docs` from the local `Protocol_V1_Soroban_testnet` and `mercury-stellar-backend` implementations. No application or contract source was changed, and no deployment or financial transaction was performed.

## Baselines

- Contracts Git HEAD: `1d333fb816454ea9d0c3614508b53d9bd0bae577`.
- Application Git HEAD: `c68077dec50ba58e11f6748dc00fd2d547581b20`.
- The contract tree also contains untracked `deploy/` and three untracked assessment/liquidation guide files. These were left in place. Executable contract bodies take precedence over comments and historical guides.
- Public app contract constants are configuration snapshots; deployed code and runtime settings were not queried.

## Corrections

- Replaced obsolete contract addresses with the 37 public address/index/alias constants from `lib/stellar-utils.ts`.
- Documented the four distinct lending markets and three separate USDC test tokens.
- Replaced old signatures with 328 current public Rust signatures, including errors in return types and parameter order.
- Added Oracle, all three protocol controllers, ControllerFacade, deployers, shared types, frontend/API, Portfolio, Copilot, and review-scope pages.
- Corrected LP valuation to the conservative `2 * min(side USD)` reserve calculation; documented live/cached valuation boundaries.
- Corrected live debt previews, floor/ceiling rounding, vToken virtual offsets, native units, partial redemption, and the 95% new-borrow utilization cap.
- Documented full-collateral liquidation, liquidator allowances, Blend exits, direct LP transfers, unpriceable plain collateral, and the separation of settlement from explicit close.
- Documented Registry bootstrap versus finalized timelocks and the post-exec/post-borrow live gate.
- Corrected supported wallets, Spot-only trading, Lite atomic/fallback behavior, chain-recovered positions, and analytics fixtures/Hubble separation.
- Preserved existing page routes and historical image assets; archived prior editorial reports explicitly. Selected screenshots were subsequently restored as historical visual guides with specific captions; outdated implementation diagrams remain excluded.

## Validation

- All **79 MDX files** compiled with `@mdx-js/mdx` 3 and `remark-gfm` 4, after removing YAML frontmatter for compilation.
- Parsed `docs.json`; all **75 navigation entries** resolve.
- Internal links/assets resolved in the initial image update. After the rendering fix, all 19 image URLs returned HTTP 200 with image content types from the local Mintlify preview.
- All **328 copied function signatures** matched reviewed Rust source. Primary references were also checked for complete method coverage and order.
- All **37 public configured values** matched the application constants exactly.
- Documented manager/pool event topic strings matched executable publish sites.
- `git diff --check` passed.

The compiler dependencies were installed under `/tmp`, not added to the docs or application. After the image rendering fix, the local Mintlify server-rendered HTML was checked on all 15 illustrated pages. No visual browser acceptance test or deployment was performed. Contract/application test suites were not run for this documentation-only change.

## Contract source map

Paths below are relative to `Protocol_V1_Soroban_testnet/contracts`. SHA-256 values identify the exact implementation contents used for the references.

| Implementation | Documentation | SHA-256 |
|---|---|---|
| `AccountManagerContract/src/account_manager.rs` | [Reference](developers/contracts/account-manager.mdx) | `93c60f2554add37dd89f2e4445143f6198c41cad7711e4d620e2fc64e79b5403` |
| `SmartAccountContract/src/smart_account.rs` | [Reference](developers/contracts/smart-account.mdx) | `3e966503cc35c9cf40d8f44749e8b96687afc731798517fb4518344713180847` |
| `lending-pool/src/pool.rs` | [Reference](developers/contracts/lending-pools.mdx) | `21f2aa32132056747e54708b7e4db08346573236fffb52537435d26a5f54911a` |
| `RiskEngineContract/src/risk_engine.rs` | [Reference](developers/contracts/risk-engine.mdx) | `b2b624bbbed7b44eb109c2770d2bb1d1585e93955acc907b7cbf118ac87e5c25` |
| `RateModelContract/src/rate_model.rs` | [Reference](developers/contracts/rate-model.mdx) | `ec5c1533a9654ed072b1249502cffbacb287d051aca722215001010a2cb473e8` |
| `v-token/src/token.rs` | [Reference](developers/contracts/vtokens.mdx) | `c92c440c6ab5a65999b42e322f748446663b9ae823fcea7c348e579a451a5392` |
| `RegistryContract/src/registry.rs` | [Reference](developers/contracts/registry.mdx) | `5d4e32bc0a97ab2838776531bb72caa915c4f7dc21520594612cc2a181283198` |
| `TrackingTokenContract/src/tracking_token.rs` | [Reference](developers/contracts/tracking-tokens.mdx) | `a2927acedbd1348e2c9e5aaf61f44eed29597555f4402f45069e669a82ee2387` |
| `ControllerFacadeContract/src/facade.rs` | [Reference](developers/contracts/controller-facade.mdx) | `0b663888b8873126bde4d0de673d8db1904815a57133c0c9a652215c38245987` |
| `BlendControllerContract/src/controller.rs` | [Reference](developers/contracts/blend-controller.mdx) | `d945ac70e4f9ebd00568643e9e3387ac18b646ed9a9a05dd99f4029a59c642c3` |
| `SoroswapControllerContract/src/controller.rs` | [Reference](developers/contracts/soroswap-controller.mdx) | `2109584d695d2d597841a86d63cc139e5d907e47bc4ba6fbca122d6fc8152f83` |
| `AquariusControllerContract/src/controller.rs` | [Reference](developers/contracts/aquarius-controller.mdx) | `9ef2d2d71e592582ecab3e87d208b2e91000d99ec8a17cd13ffae318b715c577` |
| `OracleContract/src/oracle_service.rs` | [Reference](developers/contracts/oracle.mdx) | `7d3d314e9000ef5ed6b793e644228e71561ad546d85183a95d6d022c695444b3` |
| `DeployerContract/src/deployer.rs` | [Reference](developers/contracts/deployers.mdx) | `d45fa83d9e0b619f8e53d544ad50214844220d05b39f09f17e613b0294e87f10` |
| `DeployerLPoolContract/src/pool_deployer.rs` | [Reference](developers/contracts/deployers.mdx) | `273bcf6405b84d74ec2f007364c64d20a059126d3111f98a317439b982803c1c` |

Shared definitions are documented in [Shared Types](developers/contracts/shared-types.mdx), from `vanna-common/src/types.rs`, `controller.rs`, `math.rs`, and `ttl.rs`. Contract `types.rs` and lending `events.rs` files provide data/error/event definitions used in the review.

## Application source map

| Area | Source paths within `mercury-stellar-backend` | Documentation |
|---|---|---|
| Addresses, transactions, units | `lib/stellar-utils.ts`, `lib/margin-utils.ts`, `lib/borrow-fee.ts` | Configured contracts, TypeScript integration, lending/margin guides |
| External protocols | `lib/blend-utils.ts`, `lib/aquarius-utils.ts`, `lib/soroswap-utils.ts`, corresponding hooks | Controller references, Farm and Spot |
| Wallet onboarding | `lib/wallet-adapter.ts`, `hooks/use-wallet.ts`, `contexts/privy-*`, `components/wallet` | Connect Wallet, frontend, signing flow |
| Pro/Lite flows | `store/app-mode-store.ts`, `lib/one-click-strategy.ts`, `lib/lite-positions.ts`, `components/lite-mode` | Lite Mode, transaction flow, recovered-position limits |
| Margin/Earn/Portfolio screens | `app`, `components/margin`, `hooks/use-earn.ts`, `lib/account-snapshot.ts`, `lib/margin-health.ts` | User guides and frontend reference |
| History | `lib/mercury-*.ts`, `lib/*-history-rpc.ts`, `app/api/mercury` | Mercury and event indexing |
| Analytics | `app/analytics`, `lib/analytics`, `lib/hubble`, `app/api/analytics` | Analytics guides, frontend/data limitations |
| Copilot | `app/copilot`, `app/api/copilot`, `lib/copilot`, wallet adapter | Copilot, frontend and external-service boundary |

See [Review Scope](developers/review-scope.mdx) for the reader-facing summary.

## Image usability update

Reviewed the supplied image collection and reused 19 PNGs across 15 pages. Images appear beside the relevant task or explanation, with descriptive alt text, native aspect ratios, responsive sizing, lazy loading, Mintlify native frames with click-to-zoom support, and captions distinguishing historical UI values from current behavior. The Earn supply flow now groups instructions and screenshots into steps.

Excluded outdated architecture/liquidation/lifecycle diagrams that depict unsupported perpetuals/assets, obsolete account methods, or incorrect liquidation authorization/proceeds. Also omitted charts describing a kinked rate curve, fixed maximum leverage, and contradictory Lite position estimates. Original assets were not modified.

Image placement:

| Page | Image |
|---|---|
| `learn/vtokens.mdx` | `/images/learn/vToken.png` |
| `guides/connect-wallet.mdx` | `/images/onboarding/afterConnectingWallet.png` |
| `guides/connect-wallet.mdx` | `/images/onboarding/TestnetFaucet.png` |
| `guides/analytics/overview.mdx` | `/images/analytics/Overview.png` |
| `guides/analytics/positions.mdx` | `/images/analytics/PositionLookUp.png` |
| `guides/analytics/positions.mdx` | `/images/analytics/HealthFactorHeatmap.png` |
| `guides/analytics/risk-explorer.mdx` | `/images/analytics/RiskExplorerOverview.png` |
| `guides/analytics/liquidations.mdx` | `/images/analytics/WalletEligibleForLiquidation.png` |
| `guides/margin/overview.mdx` | `/images/margin/MarginOverview.png` |
| `guides/earn/overview.mdx` | `/images/earn/EarnSectionOverview.png` |
| `guides/earn/supply.mdx` | `/images/earn/SupplyLiquidity1.png` |
| `guides/earn/supply.mdx` | `/images/earn/MyPositionTab.png` |
| `guides/earn/withdraw.mdx` | `/images/earn/WithdrawLiquidity.png` |
| `guides/trade/spot-swap.mdx` | `/images/trade/spot-selectDex.png` |
| `guides/farm/blend-pools.mdx` | `/images/farm/afterAddingLiquidity.png` |
| `guides/farm/blend-pools.mdx` | `/images/farm/removeLiquidty.png` |
| `guides/farm/overview.mdx` | `/images/farm/farmSectionOverview.png` |
| `guides/farm/leveraged-yield.mdx` | `/images/farm/LiteMode4.png` |
| `guides/farm/amm-liquidity.mdx` | `/images/farm/farmXLMMultipleAsset.png` |

The initial custom `<figure>` blocks passed MDX compilation but were omitted from the Mintlify rendered output. Replaced all 19 with native `<Frame>` components and removed the unused custom figure CSS. Verified actual `<img>` elements in server-rendered HTML on all 15 affected routes, including Earn supply and Margin overview; all 19 image URLs returned HTTP 200 with image content types. Recompiled all 79 MDX files and passed `git diff --check`. No visual browser acceptance test was performed.
