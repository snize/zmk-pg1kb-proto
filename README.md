# PG1KB Proto ZMK Firmware

PG1KB Proto (Page One Keyboard Prototype) のZMKファームウェア用モジュールです。

## 現在の実装範囲

- Seeed Studio XIAO nRF52840 Plus向けの `pg1kb_proto_right` シールド
- 5行 x 6列の右手側キーマトリクス
- PAW3222トラックボールをSPI接続のZMK pointing deviceとして有効化
- 左手側は `pg1kb_proto_left` としてシールド名だけ用意し、キー部分の足場まで作成

## 暫定ピン配置

物理配線ラベルは 1 始まりで、ファームウェア上の `row0` が `R1`、`col0` が `C1` に対応します。

| 機能 | 配線ラベル | XIAO Plusピン | ZMK/Devicetree 指定 | nRF52840ピン |
| --- | --- | --- | --- | --- |
| row0 | R1 | D0 | `&xiao_d 0` | P0.02 |
| row1 | R2 | D1 | `&xiao_d 1` | P0.03 |
| row2 | R3 | D2 | `&xiao_d 2` | P0.28 |
| row3 | R4 | D3 | `&xiao_d 3` | P0.29 |
| row4 | R5 | D4 | `&xiao_d 4` | P0.04 |
| col0 | C1 | D5 | `&xiao_d 5` | P0.05 |
| col1 | C2 | D6 | `&xiao_d 6` | P1.11 |
| col2 | C3 | D17 | `&gpio1 3` | P1.03 |
| col3 | C4 | D18 | `&gpio1 5` | P1.05 |
| col4 | C5 | D19 | `&gpio1 7` | P1.07 |
| col5 | C6 | D11 | `&gpio0 15` | P0.15 |
| PAW3222 CS | - | D7 | `&xiao_d 7` | P1.12 |
| PAW3222 SCLK | - | D8 | `NRF_PSEL(SPIM_SCK, 1, 13)` | P1.13 |
| PAW3222 SDIO | - | D9 | `NRF_PSEL(SPIM_MOSI, 1, 14)` / `NRF_PSEL(SPIM_MISO, 1, 14)` | P1.14 |
| PAW3222 MOTION | - | D10 | `&xiao_d 10` | P1.15 |

通常の XIAO nRF52840 で `&xiao_d` として扱える範囲は D0-D10 までです。マトリクス用に使っている D11/D17-D19 は XIAO nRF52840 Plus の追加ピンとして扱い、Devicetree では raw GPIO として指定します。

PAW3222 は SCLK/SDIO/CS/MOTION の 4 信号で配線します。SDIO は nRF 側の MOSI と MISO を同じ D9/P1.14 に割り当てます。

## ライセンス

このモジュール自体は MIT ライセンスです。

依存コンポーネントのライセンスは以下の通りです：

| コンポーネント | ライセンス | 備考 |
| --- | --- | --- |
| [ZMK Firmware](https://github.com/zmkfirmware/zmk) | MIT | キーボードファームウェア本体 |
| [zmk-driver-paw3222](https://github.com/sekigon-gonnoc/zmk-driver-paw3222) | Apache-2.0 | PAW3222 トラックボールドライバー。元コードは Google LLC (Zephyr Project) 著作権、sekigon-gonnoc により改変 |
