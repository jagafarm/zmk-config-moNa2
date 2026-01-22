# moNa2 ZMK Config

LiNEA40の安定設定をベースに、moNa2のハードウェアに最適化したファームウェア設定。

---

## 設定比較

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
| CPI | 1200 | - | 1200 ✅ |
| SNIPE_CPI | 500 | - | 500 ✅ |
| SMART_ALGORITHM | y | - | y ✅ |
| SCROLL_TICK | 2 | - | 2 ✅ |
| POLLING_RATE_125_SW | y | - | y ✅ |
| RUN_DOWNSHIFT_TIME | 3264 | 3264 | 3264 ✅ |
| REST1_SAMPLE_TIME | **40** | 20 | 40 ✅ |
| REST1_DOWNSHIFT_TIME | 9600 | 9600 | 9600 ✅ |
| INVERT_Y | **n** | - | y |
| INVERT_X | **n** | - | y |
| INVERT_SCROLL_X | **y** | - | n |
| INVERT_SCROLL_Y | n | - | n ✅ |
| AUTOMOUSE_TIMEOUT | 550 | - | 550 ✅ |

> **INVERT_X/Y**: moNa2とLiNEA40でトラックボールの物理的な取り付け向きが異なるため、moNa2では`n`（反転なし）に設定

---

### AML設定（overlay/dtsi）

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| automouse-layer | **1** | なし（input-processor） | 1 ✅ |
| scroll-layers | **5** | なし（input-processor） | 5 ✅ |
| snipe-layers | コメントアウト | なし | 2 |
| AML方式 | **ドライバ内蔵** ✅ | input-processor | ドライバ内蔵 ✅ |

> **AML方式**: moNa2-v2はinput-processor方式を使用していたが、安定性のためLiNEA40と同じドライバ内蔵方式を採用

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
| BT_CTLR_TX_PWR_PLUS_4 | **y（両側）** | y | なし |
| BT_PERIPHERAL_PREF_MAX_INT | なし | 12 | なし ✅ |
| ZMK_BLE_EXPERIMENTAL_CONN | **なし** | y ⚠️ | なし ✅ |
| BT_CTLR_PHY_2M | **なし** | y | なし ✅ |

> **BT_CTLR_TX_PWR_PLUS_4**: moNa2で切断問題が発生しているため、TX Powerを+4dBmに上げている。LiNEA40はデフォルト電力で安定動作するため不要
>
> **BLE_EXPERIMENTAL_CONN / PHY_2M**: moNa2-v2で有効だった実験的機能は安定性のため無効化

---

### 左側conf比較

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| CONFIG_ZMK_POINTING | **y** | なし | y ✅ |
| CONFIG_NFCT_PINS_AS_GPIOS | y | なし | y ✅ |
| CONFIG_EC11 | y | y | y ✅ |
| CONFIG_BT_CTLR_TX_PWR_PLUS_4 | **y** | y | なし |
| CONFIG_ZMK_BLE_EXPERIMENTAL_CONN | **なし** | y ⚠️ | なし ✅ |
| CONFIG_BT_CTLR_PHY_2M | **なし** | n | なし ✅ |
| CONFIG_BT_BAS | **y** | なし | なし |

> **CONFIG_ZMK_POINTING**: split pointing動作に必要。LiNEA40にはあるがmoNa2-v2にはなかった
>
> **CONFIG_BT_BAS**: Battery Service有効化（Bluetoothでバッテリー残量を通知するため）

---

### 右側overlay比較

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| ドライバcompatible | `pixart,pmw3610` | `pixart,pmw3610-alt` | `pixart,pmw3610` ✅ |
| trackball内cpi設定 | なし（conf側） | overlay内 | なし ✅ |
| trackball内invert設定 | なし（conf側） | overlay内 | なし ✅ |
| irq-gpios | **`&gpio0 2`** | `&gpio0 2` ✅ | `&xiao_d 6` |
| automouse-layer | **overlay内で設定** | なし | overlay内 ✅ |
| scroll-layers | **overlay内で設定** | なし（input-processor） | overlay内 ✅ |

> **irq-gpios**: 基板の配線が異なるため、GPIOピンが違う（moNa2: `&gpio0 2`、LiNEA40: `&xiao_d 6`）

---

### west.yml（依存関係）比較

| 項目 | このリポジトリ | moNa2-v2 | LiNEA40 |
|------|--------------|----------|---------|
| ZMK revision | **v0.2.1** | main | v0.2.1 ✅ |
| PMW3610 remote | **inorichi** | badjeff | inorichi ✅ |
| zmk-rgbled-widget | caksoylar | caksoylar | caksoylar ✅ |
| zmk-input-processor-keybind | なし | zettaface | なし ✅ |

> **inorichi vs badjeff**: inorichi版は安定性が高く、ドライバ内蔵AML方式をサポート。badjeff版（alt）はinput-processor方式

---

### レイヤー構成比較

| Layer | このリポジトリ | moNa2-v2 | LiNEA40 |
|-------|--------------|----------|---------|
| 0 | Default | default_layer | Default ✅ |
| 1 | **MOUSE (AML)** | layer_1 | MOUSE (AML) ✅ |
| 2 | MARK | layer_2 | MARK ✅ |
| 3 | CURSOR | layer_3 | CURSOR ✅ |
| 4 | FUNCTION | layer_4 | FUNCTION ✅ |
| 5 | SCROLL | **MOUSE** | SCROLL ✅ |
| 6 | WIRELESS | **SCROLL** | WIRELESS ✅ |

> **レイヤー番号**: このリポジトリはLiNEA40と同じ構成。moNa2-v2はレイヤー名と番号が異なる

---

## moNa2とLiNEA40の主な差分と理由

| 差分項目 | moNa2 | LiNEA40 | 理由 |
|----------|-------|---------|------|
| INVERT_X/Y | n | y | トラックボールの物理的な取り付け向きが異なる |
| INVERT_SCROLL_X | y | n | スクロール方向の好み調整 |
| irq-gpios | `&gpio0 2` | `&xiao_d 6` | 基板の配線が異なる |
| BT_CTLR_TX_PWR_PLUS_4 | y | なし | moNa2の切断問題対策（LiNEA40はデフォルトで安定） |
| CONFIG_BT_BAS | y | なし | バッテリー残量のBluetooth通知用 |
| snipe-layers | コメントアウト | 2 | 現在未使用のため無効化 |

---

## まとめ

✅ **安定性に関わる設定はLiNEA40踏襲**
- ZMK v0.2.1（安定版タグ）
- inorichi版PMW3610ドライバ
- BLE実験的機能無効化（EXPERIMENTAL_CONN, PHY_2M）
- CONFIG_ZMK_POINTING（左側）
- ドライバ内蔵AML方式
- レイヤー構成（MOUSE/MARK/CURSOR/FUNCTION/SCROLL/WIRELESS）

✅ **moNa2ハードウェアに最適化**
- INVERT_X/Y=n（moNa2のトラックボール配置に合わせる）
- INVERT_SCROLL_X=y
- irq-gpios=`&gpio0 2`
- BT_CTLR_TX_PWR_PLUS_4=y（切断問題対策）

✅ **moNa2-v2からの変更点**
- PMW3610ドライバ: badjeff → inorichi
- AML方式: input-processor → ドライバ内蔵
- ZMK: main → v0.2.1
- BLE実験的機能: 有効 → 無効
- レイヤー構成: LiNEA40形式に統一
