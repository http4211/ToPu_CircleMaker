# ToPu_CircleMaker

[![Blender](https://img.shields.io/badge/Blender-4.2%2B-F5792A?logo=blender&logoColor=white)](https://www.blender.org/)
[![Version](https://img.shields.io/badge/version-1.10.4-blue)](#変更履歴)
[![License](https://img.shields.io/badge/License-GPL--3.0--or--later-blue.svg)](#ライセンスとクレジット)

**円・円弧の整列、アーチ、半球、選択からの円、円形配列をまとめたBlenderアドオンです。**
Align loops to circles and arcs, and build arches, hemispheres, circles and radial arrays.

どのツールも、画面下のHUDとマウス・ホイール・キー操作で結果を見ながら調整できます。

- [日本語](#日本語)
- [English](#english)

---

## 日本語

### 主な機能

- **整列（円 / 円弧）**：選択した辺ループを円に、開いた辺列を円弧に整列します。
  - 中心の取り方は5種類。頂点が偏っていてもずれにくい **円フィット**（最小二乗）を選べます。
  - カット数をプラスにすると分割、マイナスにすると頂点を間引いてから整列します。
  - 形状に沿う（凹凸を保ったまま整列）、外周面の再構築、固定頂点（ピン）に対応します。
- **アーチ作成**：2頂点、または辺列の両端を結ぶアーチを作ります。鏡面アーチで円にもでき、アーチどうしのブリッジもできます。
- **半球を作成**：閉じた辺ループから UV / ICO / Quad の半球を作ります。
  - サイドバーのカスタムカーブで断面の形を自由に決められます。
  - リング配置「形状に最適化」は、曲がりの強い所ほどリングを多く割り当て、角にはリングを正確に合わせます。
  - 法線の向きと陰影を周囲の面になじませます。
- **選択から円作成**：頂点・辺の中点・面の中心のまわりに円を切り込みます。辺・面から作った円は、中心点のない1枚の面になります。
- **円形配列**（オブジェクトモード）：アクティブオブジェクトを円形に複製します（ディスプレイス＋配列モディファイアー）。
- 選択ツール：エッジループ / リング、隣接ループ / リング、角度制限ループ / リング。
- 日本語 / English のUIに対応します（Blenderの表示言語に自動で合わせます）。

### 対応環境

- Blender 4.2 以降（5.2 で動作確認）
- メッシュの編集モード（円形配列はオブジェクトモード）

### インストール

1. [Releases](https://github.com/http4211/ToPu_CircleMaker/releases) から ZIP をダウンロードします。
2. Blender の `編集 > プリファレンス > アドオン` で、`ディスクからインストール` を選び、ZIP を展開せずに指定します。
3. アドオン一覧で **ToPu_CircleMaker** を有効にします。

**更新するとき**は、古いバージョンを無効にして削除し、**Blender を再起動してから** 新しい ZIP を入れてください。

### 起動方法

| 方法 | 内容 |
| --- | --- |
| 右クリックメニュー | 編集モード：**ToPu Tools > 整列 / アーチ作成 / 半球を作成 / 選択から円作成**<br>オブジェクトモード：**ToPu Tools > 円形配列** |
| サイドバー（N） | **CircleMaker** タブのボタン |
| ショートカット | 初期状態では未割り当て。アドオンプリファレンスで設定できます |

右クリックメニューに出す項目は、アドオンプリファレンスで選べます。

---

### 共通の操作（HUD）

ツールを実行すると、画面下にHUDが表示されます。確定するまで、何度でも値を変えて結果を確認できます。

| 入力 | 動作 |
| --- | --- |
| マウスを左右に動かす | 「マウス」の値を変更（Shiftで微調整） |
| Ctrlを押しながらマウス / ドラッグ | 5刻みにスナップ（角度5°、割合5%、個数5、長さは区切りのよい値） |
| F | マウス左右での変更のオン・オフ |
| ホイール / Q・E | 「ホイール」の値を増減 |
| Ctrl＋ホイール / Shift＋ホイール | それぞれに割り当てた値を増減 |
| HUDのボタンをクリック | 切り替え・選択 |
| HUDの値をドラッグ / 値の上でホイール | その値を変更 |
| R | 開始時の値に戻す |
| Ctrl＋Z / Ctrl＋Shift＋Z | ツール内の元に戻す / やり直す |
| H | HUDの最小化・再表示 |
| 左クリック（HUDの外） / Enter / Space | 確定 |
| Esc / 右クリック | キャンセル（元のメッシュに戻ります） |
| 3Dマウス / 中ボタン / Alt＋ホイール / テンキー | 視点操作 |

HUDのボタンは機能ごとに色分けされています。

| 色 | 分類 |
| --- | --- |
| 青 | 円（閉じたループ）専用 |
| オレンジ | 円弧（開いた辺列）専用 |
| ティール | 中心の決め方 |
| ピンク | 形・大きさ・角度 |
| 黄 | 向き・反転・軸・回転 |
| 紫 | 分割数・面・ブリッジ |
| 黄緑 | 表面への追従・シェーディング |
| 白系 | 全体（影響度など） |

確定後も F9（最後の操作を調整）で値を変更できます。
インタラクティブ操作を使わず、従来どおり即実行にすることもできます（アドオンプリファレンス）。

---

### 整列（円 / 円弧）

閉じた辺ループは円に、開いた辺列は円弧に整列します。面を選択した場合は、選択範囲の外周を使います。

| 入力 | 動作 |
| --- | --- |
| マウス | 影響度（毎回100%から開始） |
| ホイール | カット数（−10〜10）。プラスで分割、マイナスで頂点を間引く（−1で1つおき、−2で3つに1つ） |
| Ctrl＋ホイール | 円弧の角度（角度指定をオン） |
| Shift＋ホイール | 円の直径（直径指定をオン） |
| C | 中心の取り方 |
| G | 正多角形化（頂点を等角度に配置） |
| I / D | 円：内接 / 円：直径指定 |
| A / N / V | 円弧：角度指定 / 円弧：内接 / 円弧：反転 |
| O | 外周面を再構築（円のみ） |
| S | 形状に沿う（凹凸を保ったまま整列） |

#### 中心の取り方

| 方法 | 内容 |
| --- | --- |
| 平均（重心） | 頂点位置の平均。頂点が片側に集まっているとずれます |
| バウンディングボックス | 頂点を囲む箱の中心 |
| 面積重心 | ループで囲まれた面積の重心。頂点間隔の偏りに強い |
| 辺長加重 | 辺の中点を長さで重み付けした平均。頂点の密集に強い |
| **円フィット** | 頂点に最小二乗でフィットさせた円の中心。円・円弧では最もずれにくい方法 |

半球を作成でも同じ方法を選べます（ほかに「カスタム」中心も選べます）。

#### サイドバーの補助機能

- **選択頂点をロック**：整列で動かさない頂点（ピン）を設定します。ピンがちょうど2つのときは、その2点を通る円に整列します。
- **角度取得 / 直径取得**：選択中の円弧の角度、ループの直径を測って設定に入れます。

---

### アーチ作成

2頂点、または開いた辺列の両端を結ぶアーチを作ります。

| 入力 | 動作 |
| --- | --- |
| ホイール | セグメント数 |
| マウス | 角度 |
| Ctrl＋ホイール | 回転角（弦を軸に面を回転） |
| A / HUDの X・Y・Z・法線 | 方向 |
| V | 反転 |
| M | 鏡面アーチ（反対側にも作って円にする） |
| D | エッジごとに処理 |
| P | 面を作成 |
| B | 自動ブリッジ（オンのとき L で接続方法、X で内部面を削除） |
| N | 法線の再計算 |

---

### 半球を作成

閉じた辺ループ（または面選択の外周）から半球を作ります。

| 入力 | 動作 |
| --- | --- |
| マウス | 高さ |
| ホイール | リング数（UV）/ 分割数（ICO・Quad） |
| Ctrl＋ホイール | 膨らみ係数（UV）/ 回転（ICO・Quad） |
| Shift＋ホイール | 正多角形ブレンド（UV）/ スケール（ICO・Quad） |
| T | タイプ（UV / ICO / Quad） |
| C | 中心の取り方 |
| V | 方向を反転 |
| S | 元の形状を保持 |
| U | カスタムカーブを使用（UV） |
| B | リング配置（カスタムカーブ使用時） |
| D | 内側の面を削除 |
| W | スムーズシェード |

- **カスタムカーブ**：サイドバーの **Shape Profile** パネルでカーブを編集します（X：高さ、Y：半径）。プリセットとして保存・読み込みできます。F9 でカーブを編集した場合も、再実行で形が戻りません。
- **リング配置**：「均等」は断面の長さに沿って等間隔、「形状に最適化」は曲がりの強い所ほど多く割り当てます。同じリング数で元の曲線とのずれが最も小さくなる配分で、角にはリングを正確に合わせます。
- **なじませる処理**：半球の面の向きを周囲の面とそろえます。底辺で大きく折れる所（初期値60°より大きい所）だけシャープにし、なだらかな所はなめらかにつなげます。

---

### 選択から円作成

| 選択 | 円の中心 |
| --- | --- |
| 頂点 | その頂点 |
| 辺 | 辺の中点 |
| 面 | 面の中心 |

| 入力 | 動作 |
| --- | --- |
| マウス | 半径（面の内側に収まる大きさまで） |
| ホイール | セグメント数 |
| S | 作成元（自動 / 頂点 / 辺 / 面）。自動は選択モードに合わせます |
| M | 1枚の面（辺・面から作る円を、中心点と内側の線のない1枚の面にする） |

---

### 円形配列（オブジェクトモード）

| 入力 | 動作 |
| --- | --- |
| ホイール | 数 |
| マウス | 半径（中心からの距離） |
| Ctrl＋ホイール | 全体の角度 |
| A | 回転軸 |
| D | ずらす方向 |
| C | 3Dカーソルを中心にする |

---

### アドオンプリファレンス

| 設定 | 内容 |
| --- | --- |
| 言語 | 自動（Blenderの表示言語に合わせる）／日本語／英語 |
| インタラクティブモード | オフにすると従来どおり即実行（F9で調整） |
| マウス左右で値を調整 | 開始時にマウス操作をオンにするか |
| 右クリックの動作 | キャンセル／確定 |
| HUD | スケール、解像度への追従、操作ヒント、位置 |
| ショートカット | 各ツールの起動キー（初期状態は未割り当て） |
| 右クリックメニュー | 表示する項目 |

ショートカットの変更を残すには、プリファレンスを保存してください。

### 制限事項

- アーチ作成で複数のオブジェクトにまたがって選択した場合は、HUDを使わずに即実行します（F9で調整できます）。
- 整列のカット数マイナス（頂点の間引き）は、周囲の面の頂点を溶接して行うため、近くの四角形が三角形になることがあります。
- 半球を作成の「内側の面を削除」は、ループに囲まれた面を削除します。必要な面が含まれていないか確認してから使ってください。
- すべてのメッシュの組み合わせで検証済みというわけではありません。

### ライセンスとクレジット

- **ToPu_CircleMaker**：http4211 (ToPu4211)
- HUD（`utils/hud.py`）は **ToPu_Resample** と共通です。

ライセンスは **GPL-3.0-or-later** です。全文は [LICENSE](LICENSE) を参照してください。

### 変更履歴

- **1.10**：選択から円作成（辺・面に対応、1枚の面の円）。右クリックの ToPu Tools に他の ToPu アドオンの項目があるときは区切り線で分ける。HUDの段組みを色ごと・長さをそろえて3段以内に。アーチ作成の方向を常に表示。
- **1.9**：全ツールにHUD・マウス／ホイール／キー操作。自動翻訳（日本語 / English）。中心の取り方に面積重心・辺長加重・円フィットを追加。整列のカット数マイナス（間引き）。半球のリング配置「形状に最適化」、法線のなじませ、F9でのカスタムカーブ編集。Blender 5.1 以降でサイドバーがエラーになる問題（`mesh.loop_multi_select` の廃止）を修正。半球作成で `BMVert has been removed` になる問題を修正。

---

## English

### Features

- **Align (Circle / Arc)**: align closed loops to circles and open chains to arcs.
  - Five center methods, including **Best-Fit Circle** (least squares), which stays centered even when vertices are unevenly spaced.
  - Positive cuts subdivide first; negative cuts reduce the vertices first.
  - Follow Surface, Rebuild Outer Faces and pinned vertices.
- **Create Arch**: an arch between two vertices or the ends of edge chains. Mirror it into a circle, or bridge several arches.
- **Create Hemisphere**: UV / Ico / Quad hemispheres from closed loops.
  - Draw the cross-section with the custom curve in the sidebar.
  - **Fit Shape** ring layout puts more rings where the profile bends and snaps rings to corners.
  - Normals and shading blend into the surrounding faces.
- **Circle from Selection**: cut a circle around vertices, edge midpoints or face centers. Circles from edges and faces become one clean face.
- **Radial Array** (Object Mode): Displace + Array modifiers around the object.
- Selection tools: edge loops / rings, adjacent loop / ring, angle-limited loop / ring.
- Japanese / English UI (follows Blender's language automatically).

### Requirements

Blender 4.2 or later (tested on 5.2), Mesh Edit Mode (Radial Array: Object Mode).

### Installation

1. Download the ZIP from [Releases](https://github.com/http4211/ToPu_CircleMaker/releases).
2. In `Edit > Preferences > Add-ons`, choose `Install from Disk` and select the ZIP (do not extract it).
3. Enable **ToPu_CircleMaker**.

When updating, disable and remove the old version and **restart Blender** before installing the new ZIP.

### Usage

Run the tools from **Right Click > ToPu Tools**, the **CircleMaker** sidebar tab, or a shortcut you assign in the preferences (none by default).

**Common HUD controls**: Mouse left/right = the Mouse value (Shift: fine) · Ctrl while moving or dragging = snap to steps of 5 · F = mouse control on/off · Wheel / Q,E, Ctrl+Wheel, Shift+Wheel = their values · click / drag HUD buttons · R = reset · Ctrl+Z / Ctrl+Shift+Z = in-tool undo / redo · H = hide HUD · Left click / Enter / Space = confirm · Esc / Right click = cancel · 3D mouse, MMB, Alt+Wheel and numpad navigate. Buttons are colored by kind (blue circle, orange arc, teal center, pink shape, yellow direction, purple topology, lime surface). Re-adjust with F9 after confirming.

| Tool | Mouse | Wheel | Ctrl+Wheel | Shift+Wheel | Keys |
| --- | --- | --- | --- | --- | --- |
| Align | Influence | Cuts (−10…10) | Arc angle | Circle diameter | C center, G regular, I/D circle inscribed/diameter, A/N/V arc angle/inscribed/flip, O rebuild faces, S follow surface |
| Create Arch | Angle | Segments | Rotation | – | A direction (X/Y/Z/Normal), V flip, M mirror, D per edge, P fill face, B bridge (L connection, X delete inner), N recalc normals |
| Create Hemisphere | Height | Rings / detail | Profile exponent / rotation | Regular blend / scale | T type, C center, V flip, S keep shape, U custom curve, B ring layout, D delete inner, W smooth |
| Circle from Selection | Radius | Segments | – | – | S source (auto/vertices/edges/faces), M single face |
| Radial Array | Radius | Count | Total angle | – | A axis, D direction, C 3D cursor |

### Center methods

Vertex Average, Bounding Box, Area Centroid, Edge Length Weighted and **Best-Fit Circle** (the most stable for round shapes). Create Hemisphere also offers Custom.

### Limitations

Create Arch runs immediately (no HUD) when the selection spans several objects. Negative cuts weld vertices, so nearby quads may become triangles. Not tested with every mesh setup.

### License and credits

GPL-3.0-or-later (see [LICENSE](LICENSE)). By http4211 (ToPu4211). The HUD (`utils/hud.py`) is shared with **ToPu_Resample**.
