# moNa2 ZMK Config

LiNEA40の安定設定をベースに、moNa2-v2のキーマップ・AML設定を適用したファームウェア設定。

## 変更履歴

- **2026-01-18**: BLE安定化（LiNEA40設定踏襲 + moNa2-v2キーマップ適用）

---

## 設定比較（2026-01-18時点）

### 概要

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| ZMK Version | **v0.2.1** | main | v0.2.1 ✅ |
| PMW3610 Driver | **inorichi** | badjeff(alt) | inorichi ✅ |
| Board | seeeduino_xiao_ble | xiao_ble | seeeduino_xiao_ble ✅ |
| 母艦（セントラル） | 右 | 右 | 右 ✅ |

---

### PMW3610設定（右側conf）

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| CPI | 1200 | 1200 | 1200 ✅ |
| SNIPE_CPI | 500 | - | 500 ✅ |
| SMART_ALGORITHM | y | - | y ✅ |
| SCROLL_TICK | 2 | - | 2 ✅ |
| POLLING_RATE_125_SW | y | - | y ✅ |
| RUN_DOWNSHIFT_TIME | 3264 | 3264 | 3264 ✅ |
| REST1_SAMPLE_TIME | **40** | 20 | 40 ✅ |
| REST1_DOWNSHIFT_TIME | 9600 | 9600 | 9600 ✅ |
| INVERT_Y | **n** | - | y ✅ |
| INVERT_X | **n** | - | y ⚠️ |
| AUTOMOUSE_TIMEOUT | 550 | 550 | 550 ✅ |

> **INVERT_X/Y**: moNa2とLiNEA40でトラックボールの物理的な取り付け向きが異なるため、両方 `n` (反転なし) に設定

---

### AML設定（dtsi/overlay）

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| AML_REQUIRE_IDLE_MS | 400 | 400 ✅ | - |
| aml_temp_layer timeout | 550 | 550 ✅ | - |
| excluded-positions | 17 | 17 ✅ | - |
| AML layer | 5 | 5 ✅ | 1 |
| scroller layers | 3 | 3 ✅ | 5 |
| AML方式 | **ドライバ内蔵** ✅ | input-processor | ドライバ内蔵 ✅ |

---

### エンコーダー設定（dtsi）

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| triggers-per-rotation | **8** | 10 | 8 ✅ |
| steps | 24 | 24 | 24 ✅ |

> **triggers-per-rotation**: 8=LiNEA40と同じ（少し鈍感）、10=moNa2-v2（敏感）

---

### Bluetooth設定

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| BT_CTLR_TX_PWR | **デフォルト** | +4dBm | デフォルト |
| PERIPHERAL_PREF_INT | **デフォルト** | - | デフォルト |
| ZMK_BLE_EXPERIMENTAL_CONN | 無効 | y ⚠️ | 無効 ✅ |
| BT_CTLR_PHY_2M | 無効 | y | 無効 ✅ |

> **設定**: LiNEA40と同じ「完全なデフォルト」に戻しました。接続安定性と電池持ちのバランスが最も良い状態です。

---

### 左側conf比較

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| CONFIG_ZMK_POINTING | **y** | なし | y ✅ |
| CONFIG_NFCT_PINS_AS_GPIOS | y | なし | y ✅ |
| CONFIG_BT_CTLR_TX_PWR_PLUS_4 | なし | y | なし |
| CONFIG_ZMK_BLE_EXPERIMENTAL_CONN | なし | y ⚠️ | なし ✅ |

> **CONFIG_ZMK_POINTING**: split pointing動作に必要。LiNEA40にはあるがmoNa2-v2にはなかった

---

### 右側overlay比較

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| ドライバcompatible | `pixart,pmw3610` | `pixart,pmw3610-alt` | `pixart,pmw3610` ✅ |
| trackball内cpi設定 | なし（conf側） | overlay内 | なし ✅ |
| trackball内invert設定 | なし（conf側） | overlay内 | なし ✅ |
| irq-gpios | `&gpio0 2` | `&gpio0 2` ✅ | `&xiao_d 6` |
| scroller設定 | input-processor | input-processor ✅ | ドライバ内蔵 |

---

## まとめ

✅ **安定性に関わる設定はLiNEA40踏襲**
- ZMK v0.2.1（安定版タグ）
- inorichi版PMW3610ドライバ
- BLE実験的機能無効化
- CONFIG_ZMK_POINTING（左側）

✅ **動作に関わる設定はmoNa2-v2踏襲（ただし安定性のためAMLはLiNEA40方式に変更）**
- キーマップ（combos、macros、behaviors）
- AML設定（**ドライバ内蔵方式**: input-processor版は廃止）
- ロータリーエンコーダー設定
