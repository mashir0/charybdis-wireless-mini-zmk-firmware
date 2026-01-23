# Zephyr 4.1 更新に伴う変更点（本環境）

本ファイルは ZMK の Zephyr 4.1 更新内容を前提に、**このリポジトリで変更が必要な箇所**を整理したもの。  
参考: [ZMK Zephyr 4.1 Update](https://zmk.dev/blog/2025/12/09/zephyr-4-1)

## 1. ボード名・リビジョンの更新
- **`nice_nano_v2` → `nice_nano`** に変更（Zephyr 4.1でボードリビジョン方式へ移行）
- `build.yaml` と Actions の `-b` 指定を統一
対象ファイル:
- `build.yaml`
- `.github/workflows/user_config_build.yaml`

## 2. west 更新と依存モジュールの確認
- `west update` で Zephyr 4.1 系に同期される前提
- Actions では `west update` 実行後に binding の実体を確認できるようにする  
  - 例: `/tmp/zmk-config/zephyr/dts/bindings/input/pixart,pmw3610.yaml`
対象ファイル:
- `config/west.yml`
- `.github/workflows/user_config_build.yaml`

## 3. devicetree の binding 変更（pmw3610）
Zephyr 4.1 の binding に合わせて **プロパティ名を修正**する必要がある。

- **削除対象（旧プロパティ）**
  - `irq-gpios`
  - `int-gpios`
  - `evt-type`
  - `x-input-code`
  - `y-input-code`
  - `cpi`

- **追加/変更対象（新プロパティ）**
  - `motion-gpios`（必須）
  - `zephyr,axis-x` / `zephyr,axis-y`（必須）
  - `res-cpi`
  - `smart-mode` / `invert-x` / `invert-y`（必要に応じて）

対象ファイル:
- `boards/shields/charybdis-bt/charybdis_pmw3610.dtsi`
- `boards/shields/charybdis-dongle/charybdis_pmw3610.dtsi`

## 4. Kconfig 未定義エラー対策
Zephyr 4.1 では **Kconfig シンボルが整理/削除**されているため、未定義エラーに注意。

### 4.1 PMW3610 の Kconfig
`PMW3610_*` 系の Kconfig が未定義になる場合、**Kconfig ではなく devicetree 側に寄せる**。

対象ファイル:
- `config/charybdis_right.conf`

### 4.2 ZMK Studio の依存
`CONFIG_ZMK_STUDIO_TRANSPORT_BLE_PREF_LATENCY` は `ZMK_STUDIO_RPC` 依存。  
未設定の場合は警告の原因になるため、**必要なときだけ有効化**する。

対象ファイル:
- `boards/shields/charybdis-bt/charybdis_left.conf`
- `boards/shields/charybdis-bt/charybdis_right.conf`

## 5. その他（Zephyr 4.1 の一般的変更点）
本リポジトリでは直接影響が少ないが、将来的に関係する可能性がある項目:

- **NFC ピンの GPIO 化**  
  Kconfig から devicetree へ移行
- **DC/DC 設定の移行**  
  Kconfig から devicetree の `&reg0` / `&reg1` へ移行
- **board extensions**  
  upstream board の拡張は `boards/extensions` で対応可能

## 6. ビルド検証
1. Actions で `west update` 後の binding を確認
2. devicetree エラーが解消されていることを確認
3. Kconfig 警告が出ないことを確認
4. 右側・左側のビルドが通ることを確認
5. 実機でトラックボール動作確認
