# Palo Alto 基礎

## PAN-OS の基礎

Palo Alto Networks のファイアウォールでは、CLIにログインすると最初は**Operationalモード（操作モード）**から始まる。

### Operationalモード

プロンプトが `>` になっている状態。

```text
>
```

主に、現在の設定や機器の状態を確認するためのモード。

Cisco機器でいう `show` コマンドを実行するモードに近い。

---

### 基本的な確認コマンド

#### システム情報の確認

```text
> show system info
```

機器の基本情報を確認する。

確認できる情報の例：

* Hostname
* IPアドレス
* MACアドレス
* PAN-OSバージョン
* シリアル番号
* ライセンス関連情報

まず機器にログインしたら確認しておきたい基本コマンド。

---

#### インターフェイスの確認

```text
> show interface all
```

すべてのインターフェイス情報を確認する。

特定のインターフェイスを確認する場合：

```text
> show interface ethernet1/1
```

Tunnelインターフェイスを確認する場合：

```text
> show interface tunnel
```

主に以下のような情報を確認できる。

* IPアドレス
* Link状態
* Forwarding状態
* Zone
* VLAN Tag
* インターフェイス情報

---

#### ルーティングテーブルの確認

```text
> show routing route
```

現在のルーティングテーブルを確認する。

Cisco IOSでいう以下のコマンドに近い。

```text
show ip route
```

Static Routeや接続されているネットワーク、経路情報などを確認するときに使用する。

---

## Configurationモード

設定を変更する場合は、Operationalモードから以下を実行する。

```text
> configure
```

Configurationモードに入ると、プロンプトが `>` から `#` に変わる。

```text
#
```

Cisco機器のConfigurationモードに近い。

---

### 設定の投入

設定を変更するときは `set` コマンドを使用する。

```text
# set ...
```

例：

```text
# set deviceconfig system hostname PA-TEST
```

この時点では、まだ実際の動作には反映されていない。

Palo Altoでは、Configurationモードで変更した設定は**Candidate Configuration**として保持される。

---

### 設定の反映

設定変更後、以下を実行する。

```text
# commit
```

`commit` を実行することで、Candidate Configurationの内容が実際の設定として反映される。

つまり基本的な流れは、

```text
> configure
# set ...
# commit
```

となる。

---

### Operationalモードへ戻る

Configurationモードから戻る場合：

```text
# exit
```

Operationalモードに戻る。

```text
>
```

---

## 基本操作まとめ

```text
> show system info
```

機器の基本情報を確認。

```text
> show interface all
```

インターフェイスを確認。

```text
> show routing route
```

ルーティングテーブルを確認。

```text
> configure
```

Configurationモードへ移行。

```text
# set ...
```

設定を変更。

```text
# commit
```

設定を反映。

```text
# exit
```

Operationalモードへ戻る。

---

## Ciscoとの簡単な比較

| Palo Alto            | Cisco IOS            | 用途       |
| -------------------- | -------------------- | -------- |
| `>`                  | `>` / `#`            | 状態確認     |
| `configure`          | `configure terminal` | 設定モードへ移行 |
| `set ...`            | 各種設定コマンド             | 設定変更     |
| `commit`             | 基本的に不要               | 設定を実際に反映 |
| `show routing route` | `show ip route`      | ルーティング確認 |
| `exit`               | `exit`               | モードを抜ける  |

### Palo Altoで特に重要なポイント

Ciscoとの大きな違いの一つが**commit**。

```text
set → commit
```

まで行って、初めて設定が反映される。

そのため、

> **設定を入れただけでは終わりではなく、最後にcommitする**

ということを最初に覚えておく。
