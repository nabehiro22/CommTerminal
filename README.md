# CommTerminal

A simple serial and TCP communication terminal for Windows.  
Built with [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — GPU-accelerated rendering keeps CPU usage low, and falls back gracefully on systems without a dedicated GPU.

---

## Features

- UART (serial) and TCP communication (client / server)
- Send and receive text and binary data in the same session
- Mixed text/binary data support — send hex bytes alongside ASCII strings
- Text and hex display modes — switch between readable text and raw byte values
- Copy log lines or selected text via Ctrl+C or right-click context menu
- Color-coded log — configurable separate colors for sent and received data
- Status bar showing current connection settings at a glance
- Window position, size and maximized state restored on the next launch
- Language switching (English / Japanese) via the settings screen
- GPU-accelerated UI via GPUI (falls back to CPU rendering if no GPU is available)

## Built With

- [Rust](https://www.rust-lang.org/) — systems programming language
- [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — GPU-accelerated UI framework

## Requirements

- Windows 10 or later
- No additional runtime or GPU required

## Usage

### Sending mixed text and binary data

Wrap hex bytes in `{}` with space-separated 2-digit hex values:

```
Hello{0D 0A}
{41 42 43}
```

- Plain text outside `{}` is sent as UTF-8
- `{0D 0A}` is interpreted as raw bytes (e.g. CR LF)
- `{{` and `}}` are escape sequences for literal `{` and `}`

### Display modes

Click **Text** or **Hex** to switch the log display:

- **Text** — received bytes are decoded as UTF-8. Bytes that cannot form valid UTF-8 are shown as `�` (U+FFFD). Each entry ends with `↵` for a normal terminator, or `│` for a timeout.
- **Hex** — all bytes are shown as space-separated 2-digit hex values (e.g. `41 42 43 0D 0A`).

Switching between the two modes clears the current text selection. The rendered text differs between modes, so a selection made in one mode would not cover the same data in the other.

### Copying log data

- **Ctrl+A** — selects every line currently held in the log
- **Ctrl+C** — copies the selected text in the log area
- **Right-click** — opens a context menu with "Copy Selection" and "Copy Line"

While dragging to select text, moving the cursor outside the log area scrolls the view automatically, so a selection can be extended beyond what is currently visible.

### Log capacity

The log retains up to **1,000 lines**. When this limit is reached, the oldest lines are removed automatically to make room for new ones.

### Window position and size

The window position, size and maximized state are saved when the application is closed, and restored the next time it starts. The monitor the window was displayed on is recorded as well, so on a multi-monitor setup the window reopens on the same monitor.

If that monitor is no longer connected, or the saved position no longer fits on the screen, the window opens at the default position on the primary monitor.

The state is written when the window is closed normally. It is not saved if the process is terminated forcibly, for example from Task Manager.

### Settings

Click **Settings** to open the settings panel. Available options depend on the communication mode:

**Serial**

| Setting | Description |
|---|---|
| COM Port | Serial port to use |
| Baud Rate | Communication speed (bps) |
| Parity | None / Odd / Even |
| Stop Bits | 1 / 2 |
| Data Bits | Data length in bits |
| Flow Control | None / Software (XON/XOFF) / Hardware (RTS/CTS) |
| Timeout (ms) | Receive timeout. A line is finalized when no new data arrives within this period |
| Send Terminator | Bytes appended to each transmission (None / CR / LF / CR+LF / ETX / EOT) |
| Recv Terminator | Byte sequence that marks the end of a received line |
| Send Color | Log color for sent data |
| Recv Color | Log color for received data |

**TCP Server**

| Setting | Description |
|---|---|
| Bind IP | IP address to listen on (`0.0.0.0` listens on all interfaces) |
| Listen Port | Port number to accept connections on |
| Timeout (ms) | Receive timeout |
| Send / Recv Terminator | Same as serial |
| Send / Recv Color | Same as serial |

The server accepts **one client at a time**. After a client disconnects, it automatically resumes listening for the next connection.

**TCP Client**

| Setting | Description |
|---|---|
| Host | IP address or hostname of the server |
| Port | Port number to connect to |
| Connect Timeout (ms) | Maximum time to wait for a connection to be established |
| Timeout (ms) | Receive timeout |
| Send / Recv Terminator | Same as serial |
| Send / Recv Color | Same as serial |

### Receive timeout

When a terminator is configured, a line is finalized when the terminator byte sequence is received.  
When no terminator is configured, or when partial data arrives without a terminator, the line is finalized after the configured timeout period with no new data. Lines finalized by timeout are marked with `│` instead of `↵`.

### Settings file

Settings are saved automatically to `config.toml` in the same folder as the executable. The window position, size, maximized state and the monitor in use are stored in the same file. You can copy or back up this file to preserve your configuration.

### Language

The display language can be changed in the settings panel. Currently supported: **English** and **Japanese**.

The language strings are defined in `en.toml` and `ja.toml` in the application folder. You can edit these files to customize messages or add translations for other languages.

### Multiple instances

Only one instance of CommTerminal can run at a time. If you launch a second instance, the existing window is brought to the front and the new instance exits immediately.

## License

Copyright 2026 Hiroki Watanabe  
Licensed under the [Apache License, Version 2.0](LICENSE).

This software is provided "as is", without warranty of any kind.  
The author is not responsible for any damages or issues arising from its use.

## Contributing / Bug Reports

If you find a bug or have a question, please open an [Issue](https://github.com/nabehiro22/CommTerminal/issues).

## Acknowledgements

- [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — Apache 2.0
- [gpui-component](https://github.com/longbridge/gpui-component) — Apache 2.0

---

# CommTerminal（日本語）

Windows向けのシンプルなシリアル・TCP通信ターミナルです。  
[GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) によるGPU描画でCPU負荷を低減します。GPU未搭載の環境でもCPU描画にフォールバックして動作します。

---

## 機能

- UARTシリアル通信・TCP通信（クライアント／サーバー）に対応
- 文字列とバイナリデータの送受信が可能
- 文字列とバイナリが混在するデータの送受信に対応
- 文字表示・16進表示の切り替えが可能
- ログの選択テキストまたは行単位でのコピー（Ctrl+C・右クリックメニュー）
- 送信データと受信データをそれぞれ個別に色設定できるカラーログ
- 現在の接続設定をひと目で確認できるステータスバー
- 終了時のウィンドウ位置・サイズ・最大化状態を次回起動時に復元
- 設定画面から表示言語を切り替え可能（日本語・英語）
- GPUIによるGPU描画（GPU未搭載の場合はCPU描画で動作）

## 開発言語・フレームワーク

- [Rust](https://www.rust-lang.org/) — システムプログラミング言語
- [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — GPU描画UIフレームワーク

## 動作環境

- Windows 10 以降
- 追加のランタイムやGPUは不要

## 使い方

### 文字列とバイナリを混在して送信する

`{}`で囲んだ中に16進数2桁をスペース区切りで記述します。

```
Hello{0D 0A}
{41 42 43}
```

- `{}`の外のテキストはそのままUTF-8で送信されます
- `{0D 0A}` のように`{}`内はバイト値として解釈されます（例：CR LF）
- `{{`と`}}`はそれぞれ`{`と`}`のエスケープ表記です

### 表示モード

**文字**・**16進**ボタンで表示を切り替えられます。

- **文字** — 受信バイト列をUTF-8として表示します。有効なUTF-8に変換できないバイトは `�`（U+FFFD）で表示されます。各行の末尾は終端文字による区切りの場合 `↵`、タイムアウトによる区切りの場合 `│` が付きます。
- **16進** — すべてのバイトを16進数2桁のスペース区切りで表示します（例：`41 42 43 0D 0A`）。

表示モードを切り替えると選択範囲は解除されます。モードによって表示される文字列が異なるため、一方で選択した範囲がもう一方では同じデータを指さなくなるためです。

### ログのコピー

- **Ctrl+A** — ログに保持されている全行を選択します
- **Ctrl+C** — ログエリアで選択したテキストをコピーします
- **右クリック** — 「選択部をコピー」「行をコピー」のコンテキストメニューが表示されます

ドラッグで範囲選択している間にマウスカーソルをログ表示エリアの外へ動かすと、表示が自動的にスクロールします。画面に見えていない範囲まで続けて選択できます。

### ログの最大行数

ログは最大 **1,000行** まで保持されます。上限に達すると、新しいデータが来るたびに古い行から順に削除されます。

### ウィンドウの位置とサイズ

終了時のウィンドウ位置・サイズ・最大化状態を保存し、次回起動時に復元します。表示していたモニターも記録するため、マルチモニター環境でも同じモニターに復元されます。

該当のモニターが接続されていない場合や、保存された位置が画面に収まらない場合は、プライマリモニターの既定位置に表示します。

保存はウィンドウを通常の操作で閉じたときに行われます。タスクマネージャーからの強制終了などでは保存されません。

### 設定

**設定**ボタンをクリックすると設定パネルが開きます。表示される項目は通信方式によって異なります。

**シリアル通信**

| 設定項目 | 説明 |
|---|---|
| COMポート | 使用するシリアルポート |
| 通信速度 | 通信速度（bps） |
| パリティ | None / Odd / Even |
| ストップビット | 1 / 2 |
| データ長 | データのビット長 |
| フロー制御 | None / Software (XON/XOFF) / Hardware (RTS/CTS) |
| タイムアウト(ms) | 受信タイムアウト。最後の受信からこの時間が経過すると行を確定します |
| 送信終端 | 送信データの末尾に付加するバイト列（None / CR / LF / CR+LF / ETX / EOT） |
| 受信終端 | 受信行の区切りを示すバイト列 |
| 送信色 | 送信データのログ表示色 |
| 受信色 | 受信データのログ表示色 |

**TCPサーバ**

| 設定項目 | 説明 |
|---|---|
| 待ち受けIP | 待ち受けに使用するIPアドレス（`0.0.0.0` は全インタフェースで待ち受け） |
| 待ち受けポート | クライアントの接続を受け付けるポート番号 |
| タイムアウト(ms) | 受信タイムアウト |
| 送信終端・受信終端 | シリアルと同様 |
| 送信色・受信色 | シリアルと同様 |

同時接続は **1台のみ**です。クライアントが切断すると自動的に次の接続の待ち受けを再開します。

**TCPクライアント**

| 設定項目 | 説明 |
|---|---|
| 接続先ホスト | 接続先のIPアドレスまたはホスト名 |
| 接続先ポート | 接続先のポート番号 |
| 接続タイムアウト(ms) | 接続が確立するまでの最大待ち時間 |
| タイムアウト(ms) | 受信タイムアウト |
| 送信終端・受信終端 | シリアルと同様 |
| 送信色・受信色 | シリアルと同様 |

### 受信タイムアウト

終端文字が設定されている場合、終端バイト列を受信した時点で行を確定します。  
終端文字が設定されていない場合、または終端文字が来ないまま一定時間が経過した場合、設定したタイムアウト時間が経過した時点で行を確定します。タイムアウトで確定した行は末尾に `↵` の代わりに `│` が表示されます。

### 設定ファイル

設定は実行ファイルと同じフォルダの `config.toml` に自動保存されます。ウィンドウの位置・サイズ・最大化状態・表示していたモニターも同じファイルに保存されます。このファイルをコピーまたはバックアップすることで設定を保持できます。

### 言語設定

設定パネルから表示言語を切り替えられます。現在対応している言語は**日本語**と**英語**です。

表示文字列は実行ファイルと同じフォルダの `ja.toml`（日本語）と `en.toml`（英語）で定義されています。これらのファイルを編集することでメッセージをカスタマイズしたり、他の言語を追加したりできます。

### 多重起動の禁止

CommTerminalは同時に1つのインスタンスしか起動できません。2つ目を起動しようとすると、既存のウィンドウが前面に表示されて新しいインスタンスは即座に終了します。

## ライセンス

Copyright 2026 Hiroki Watanabe  
[Apache License, Version 2.0](LICENSE) のもとで公開しています。

本ソフトウェアは現状のまま（"as is"）提供されます。  
使用によって生じたいかなる損害・問題についても、作者は責任を負いません。

## バグ報告・質問

バグの報告や質問は [Issues](https://github.com/nabehiro22/CommTerminal/issues) からお願いします。

## 使用ライブラリ

- [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — Apache 2.0
- [gpui-component](https://github.com/longbridge/gpui-component) — Apache 2.0