# integrity-lab

A **frozen, unmaintained** mirror of code removed from
[integrity-core](https://github.com/XibalbaTechSol/integrity-core) during the Phase A1 restructure
(integrity-core `docs/EXECUTION_PLAN.md`, approved 2026-09-28). Nothing here is built, tested,
deployed or supported. It exists so the removed work stays readable, with its history, without
living in the active codebase.

## Provenance

- **Source snapshot:** integrity-core `main` at `761e019ffef0232369268612a954762d633c8fea`
  (2026-09-28), the last commit before the cut. The whole pre-cut tree stays available there.
- **History:** commits are integrity-core's own, filtered with `git filter-repo --paths-from-file`
  to exactly the files listed below, so `git log` and `git blame` keep original authors and dates.
- **Removal commits** on integrity-core's `claude/quirky-wright-g7yl7g` branch (PR #101):
  `ff8e22c` (A4, adapter-registry hook), `e1bf9ff` (SDK), `ca1dd39` (contracts), `10c4d52`
  (oracle and CLI), and the stage 4 commit (integrity-zkp).
- **Moved, not cut:** the dashboard, userapi and demo went to
  [integrity-console](https://github.com/XibalbaTechSol/integrity-console) and are not duplicated here.

## Partial cuts (code removed from files that stayed in integrity-core)

A path filter cannot carry these; read them at the source snapshot above.

- `integrity-oracle/backend/src/{handlers,chain,db,openapi,routes,config,lib,main,error}.rs`:
  the `/v1/markets*`, `/v1/stats`, `/v1/governance/proposals` and
  `/v1/agent/{id}/{stake,credit,contracts}` routes, their chain reads and market cache, the wallet's
  `open_positions`, and the bb-backed ZK verifier wiring.
- `contracts/script/Deploy.s.sol`: genesis deployment of markets, the capital pool,
  `IntegrityGovernance` and `UltraPlonkVerifier`. Replaced by `RejectAllZkVerifier` and admin
  handoff to the configured governance address.
- `contracts/src/kernel/IntegrityKernel.sol`: the adapter-registry hook (constructor arguments 10–11
  and the gas-bounded adapter call).
- Root `Makefile`, `.github/workflows/ci.yml`, `docker-compose.yml`: the zkp, dashboard and
  userapi targets, jobs and services.

## Cut-path manifest

91 files.

### ZK proving (retired)

- `bbup.sh`
- `contracts/src/oracle/UltraPlonkVerifier.sol`
- `contracts/test/UltraPlonkVerifier.t.sol`
- `contracts/test/fixtures/ultraplonk/proof.bin`
- `contracts/test/fixtures/ultraplonk/public_inputs.bin`
- `integrity-oracle/backend/src/zk.rs`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/circuit/Nargo.toml`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/circuit/src/main.nr`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/proof`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/public_inputs`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/tampered_public_inputs`
- `integrity-oracle/backend/tests/fixtures/zk_smoke/vk`
- `integrity-sdk/circuits/poc_commitment/Nargo.toml`
- `integrity-sdk/circuits/poc_commitment/src/main.nr`
- `integrity-sdk/integrity_sdk/prover.py`
- `integrity-sdk/tests/unit/test_prover.py`
- `integrity-zkp/.gitignore`
- `integrity-zkp/Makefile`
- `integrity-zkp/Nargo.toml`
- `integrity-zkp/README.md`
- `integrity-zkp/circuit/Nargo.toml`
- `integrity-zkp/circuit/Prover.toml`
- `integrity-zkp/circuit/src/main.nr`
- `integrity-zkp/generated/UltraPlonkVerifier.sol`
- `integrity-zkp/tools/commitment_calc/Nargo.toml`
- `integrity-zkp/tools/commitment_calc/Prover_a8b3b22ff91d.toml`
- `integrity-zkp/tools/commitment_calc/src/main.nr`
- `noirup.sh`

### Prediction markets and A2A capital pool

- `contracts/script/DeployMarkets.s.sol`
- `contracts/src/markets/A2ACapitalPool.sol`
- `contracts/src/markets/IntegrityMarket.sol`
- `contracts/src/markets/MarketFactory.sol`
- `contracts/test/markets/A2ACapitalPool.t.sol`
- `contracts/test/markets/IntegrityMarket.t.sol`
- `integrity-cli/integrity_cli/abis/A2ACapitalPool.json`
- `integrity-cli/integrity_cli/abis/IntegrityMarket.json`
- `integrity-cli/integrity_cli/abis/MarketFactory.json`
- `integrity-sdk/integrity_sdk/abis/A2ACapitalPool.json`
- `integrity-sdk/integrity_sdk/abis/IntegrityMarket.json`
- `integrity-sdk/integrity_sdk/abis/MarketFactory.json`
- `integrity-sdk/integrity_sdk/markets.py`
- `integrity-sdk/tests/test_markets.py`

### Licence layer (ERC-6551 licences, paymaster, terms)

- `contracts/script/DemoLicenceConsumption.s.sol`
- `contracts/script/DeployLicenceReference.s.sol`
- `contracts/script/SubmitSponsoredLicenceUserOp.s.sol`
- `contracts/script/VerifyLicenceReference.s.sol`
- `contracts/src/licence/IERC6551.sol`
- `contracts/src/licence/ILicenceDelegationView.sol`
- `contracts/src/licence/ILicenceHook.sol`
- `contracts/src/licence/ILicenceTermsHook.sol`
- `contracts/src/licence/LicenceAccount.sol`
- `contracts/src/licence/LicenceEconomy.sol`
- `contracts/src/licence/LicencePaymaster.sol`
- `contracts/src/licence/LicenceTermsPolicy.sol`
- `contracts/src/licence/LicenceToken.sol`
- `contracts/src/licence/ReputationFloorLicenceHook.sol`
- `contracts/test/licence/ConsumeWithIntent.t.sol`
- `contracts/test/licence/Erc6551RegistryIntegration.t.sol`
- `contracts/test/licence/LicenceAccount.t.sol`
- `contracts/test/licence/LicenceAccount4337.t.sol`
- `contracts/test/licence/LicenceAccountHook.t.sol`
- `contracts/test/licence/LicenceEconomy.t.sol`
- `contracts/test/licence/LicencePaymaster.t.sol`
- `contracts/test/licence/LicenceTermsPolicy.t.sol`
- `contracts/test/licence/ProtocolFeeSettlement.t.sol`

### On-chain adapter registry (adapters are compile-time pack transducers now)

- `contracts/script/AdapterAdmissionSuite.s.sol`
- `contracts/script/adapter-admission-vectors/spend-budget-example.json`
- `contracts/src/registry/AdapterRegistry.sol`
- `contracts/src/registry/IAdapter.sol`
- `contracts/src/registry/ReputationFloorAdapter.sol`
- `contracts/src/registry/SpendBudgetAdapter.sol`
- `contracts/test/halmos/KernelPropertiesRegistryEnabled.t.sol`
- `contracts/test/kernel/IntegrityKernelRegistryHook.t.sol`
- `contracts/test/registry/AdapterAdmissionSuite.t.sol`
- `contracts/test/registry/AdapterRegistry.t.sol`
- `contracts/test/registry/ReputationFloorAdapter.t.sol`
- `contracts/test/registry/SpendBudgetAdapter.t.sol`

### Cross-chain bridge (CCIP)

- `contracts/src/oracle/CCIPReputationBridge.sol`
- `contracts/test/CCIPReputationBridge.t.sol`

### On-chain governance

- `contracts/script/DeployXnsGovernance.s.sol`
- `contracts/src/oracle/IntegrityGovernance.sol`
- `contracts/test/IntegrityGovernance.t.sol`

### SDK MCP server, auto-hook, duplicate OPA clients

- `integrity-sdk/integrity_sdk/__main__.py`
- `integrity-sdk/integrity_sdk/integrations/auto_hook.py`
- `integrity-sdk/integrity_sdk/mcp_server.py`
- `integrity-sdk/integrity_sdk/opa_client.py`
- `integrity-sdk/integrity_sdk/policy/__init__.py`
- `integrity-sdk/integrity_sdk/policy/opa_client.py`
- `integrity-sdk/tests/test_mcp_server_signing_boundary.py`
- `integrity-sdk/tests/unit/test_auto_hook.py`

### Stray files

- `docker-compose.yml.before-opa-policy-selection`
