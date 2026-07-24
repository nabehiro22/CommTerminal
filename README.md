# CommTerminal

A simple serial and TCP communication terminal for Windows.  
Built with [GPUI](https://github.com/zed-industries/zed/tree/main/crates/gpui) — GPU-accelerated rendering keeps CPU usage low, and falls back gracefully on systems without a dedicated GPU.

---

## Features

- UART (serial) and TCP communication (client / server)
- Send and receive text and binary data in the same session
- Mixed text/binary data support — send hex bytes alongside ASCII strings
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