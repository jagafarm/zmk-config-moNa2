設定比較（2026-01-18時点）

このリポジトリはLiNEA40の安定設定をベースに、moNa2-v2のキーマップ・AML設定を適用しています。

### PMW3610設定

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| ZMK Version | v0.2.1 | main | v0.2.1 ✅ |
| PMW3610 Driver | inorichi | badjeff(alt) | inorichi ✅ |
| CPI | 1200 | 1200 | 1200 ✅ |
| SNIPE_CPI | 500 | - | 500 ✅ |
| SMART_ALGORITHM | y | - | y ✅ |
| RUN_DOWNSHIFT_TIME | 3264 | 3264 | 3264 ✅ |
| REST1_SAMPLE_TIME | 40 | 20 | 40 ✅ |
| REST1_DOWNSHIFT_TIME | 9600 | 9600 | 9600 ✅ |
| AUTOMOUSE_TIMEOUT | 550 | 550 | 550 ✅ |

### AML設定

| 項目 | このリポジトリ | moNa2-v2 |
|------|--------------|----------|
| AML_REQUIRE_IDLE_MS | 400 | 400 ✅ |
| aml_temp_layer timeout | 550 | 550 ✅ |
| excluded-positions | 17 | 17 ✅ |
| AML layer | 5 | 5 ✅ |
| scroller layers | 3 | 3 ✅ |

### Bluetooth設定

| 項目 | このリポジトリ | moNa2-v2 |
|------|--------------|----------|
| BLE_EXPERIMENTAL_CONN | 無効 | 有効 |
| BT_CTLR_PHY_2M | 無効 | 有効 |

> **まとめ**: 安定性に関わる設定はLiNEA40踏襲、動作に関わる設定（キーマップ・AML）はmoNa2-v2踏襲
