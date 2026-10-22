# 🐳 Docker コマンド操作チートシート【新人向け】

> うさうさ研修工房｜対象：Docker初心者｜環境：Windows + WSL2（Ubuntu）／VSCode／PowerShell

## 0. このチートシートの使い方

1. **「1. 最初に知ること」** で言葉を理解する
2. **「2. 必須コマンドTOP30」** を最優先で覚える（S＝初日、A＝1週間、B＝慣れたら）
3. 困ったら **「3. 状態確認」** の診断フローを上から順に実行する
4. 自分が今使っている画面を **「4. 環境別の違い」** で確認する（Ubuntu／VSCode／PowerShell）
5. **「10. 練習ハンズオン」** を上から順に打って体で覚える
6. Ubuntu・VSCode・PowerShell・CMD・Windows自体の使い方は **「12〜16」**、Claude Codeと自作スキルのまとめ方は **「17〜18」** を見る

> 💡 **コピペするときの合言葉**：「今どの画面？」を毎回確認。bash用のコマンドをPowerShellに貼ると動かないことがあります。

## 1. 最初に知ること（5つの言葉）

| 言葉 | たとえ | 説明 |
|---|---|---|
| イメージ | 料理のレシピ＋材料セット | コンテナの設計図。変更できない「ひな形」 |
| コンテナ | 作った料理 | イメージから作られる実行中の箱。消しても何度でも作れる |
| Dockerfile | レシピ本 | イメージの作り方を書いたテキストファイル |
| ボリューム | 冷蔵庫（保存場所） | コンテナを消してもデータを残す仕組み |
| Compose | 献立表 | 複数コンテナをまとめて起動する設定（compose.yaml） |

```
Dockerfile ──build──▶ イメージ ──run──▶ コンテナ
                         ▲                  │
               pull / push（Docker Hub）    └─ stop / start / rm
```

## 2. 必須コマンドTOP30（ここから覚える）

ランク：**S**＝初日に覚える ／ **A**＝1週間で覚える ／ **B**＝慣れたら

| 順 | コマンド | 一言で言うと | いつ使う？ | 覚え方・合言葉 | ランク |
|---|---|---|---|---|---|
| 1 | `docker --version` | バージョン確認 | インストール直後／トラブル報告時 | 「まず version」 | S |
| 2 | `docker run --rm hello-world` | 動作確認 | 最初の1回／環境が壊れた疑い | 「Hello が出ればOK」 | S |
| 3 | `docker pull イメージ名:タグ` | イメージを取ってくる | 新しいソフトを使う前 | pull＝引っ張る（ダウンロード） | S |
| 4 | `docker images` | 手元のイメージ一覧 | 何を持っているか確認 | 「images＝在庫」 | S |
| 5 | `docker run -d --name 名前 -p 8080:80 イメージ` | 作って起動（定番形） | コンテナを動かす時はほぼこれ | 「ど・な・ぽ」＝-d／--name／-p | S |
| 6 | `docker ps` | 動いているコンテナ一覧 | 起動できたか確認 | ps＝今いる人 | S |
| 7 | `docker ps -a` | 止まっているものも全部 | 消し忘れ・Exited の確認 | -a＝all（全部） | S |
| 8 | `docker logs -f 名前` | ログを追いかけ表示 | 動かない・エラーの時の最初の一手 | logs＝コンテナの独り言 | S |
| 9 | `docker exec -it 名前 bash` | 動いているコンテナの中に入る | 中を見たい・コマンドを試したい | 「えぐぜっく・あいてぃー」入る時の呪文 | S |
| 10 | `docker stop 名前` | 止める | 作業を終える | stop＝やさしく止める | S |
| 11 | `docker start 名前` | 止めたものを動かす | もう一度使う | start＝既存を動かす（runは新規） | S |
| 12 | `docker rm 名前` | コンテナを消す | 不要になった／名前の重複 | rm＝remove。止めてから消す | S |
| 13 | `docker rmi イメージ名` | イメージを消す | 容量を空けたい | rmi＝remove image | A |
| 14 | `docker restart 名前` | 再起動 | 設定変更後 | stop→startの一手 | A |
| 15 | `docker build -t 名前:版 .` | Dockerfileからイメージ作成 | 自作アプリをDocker化 | 最後の「.」を忘れない | A |
| 16 | `docker run --rm -it イメージ sh` | お試し用の使い捨て起動 | ちょっと中を試したい | --rm＝使い捨て、残骸なし | A |
| 17 | `-v "$(pwd)":/app` | 手元のフォルダを見せる | ソースや設定ファイルを共有 | 左＝自分、右＝コンテナ | A |
| 18 | `-e KEY=VALUE` | 環境変数を渡す | DBのパスワード設定など | e＝environment | A |
| 19 | `docker cp` | ファイルをコピー | コンテナ内ファイルを取り出す／入れる | コピー元 → コピー先 | A |
| 20 | `docker inspect 名前` | 詳細情報（JSON） | ポート・マウント・IPの確認 | inspect＝健康診断の詳細版 | A |
| 21 | `docker stats --no-stream` | CPU・メモリを1回表示 | 重い・落ちる原因の調査 | stats＝コンテナの体力 | A |
| 22 | `docker volume ls` | ボリューム一覧 | データが残っているか確認 | volume＝倉庫 | A |
| 23 | `docker network ls` | ネットワーク一覧 | コンテナ同士が繋がらない時 | network＝道路 | A |
| 24 | `docker compose up -d` | まとめて起動 | 複数コンテナ構成 | up＝全員起こす | A |
| 25 | `docker compose ps` | Composeの状態確認 | まとめて起動した後 | compose版の ps | A |
| 26 | `docker compose logs -f` | Composeのログ追跡 | 動かない時 | compose版の logs | A |
| 27 | `docker compose down` | まとめて停止＋削除 | 片付け | down＝全員寝かせて片付け | A |
| 28 | `docker system df` | 容量の使用状況 | ディスクが足りない | df＝disk free と同じ発想 | B |
| 29 | `docker system prune` | 未使用をまとめて削除 | お掃除（内容を読んでから） | prune＝剪定（慎重に） | B |
| 30 | `docker コマンド --help` | そのコマンドの使い方 | 忘れた時 | 困ったら --help | S |

### 2-1. セットで覚える動詞

| グループ | 流れ | 合言葉 |
|---|---|---|
| コンテナの一生 | run → ps → stop → start → rm | 作る・見る・止める・動かす・消す |
| 見る4点セット | ps → logs → inspect → stats | 居る？・何て言ってる？・設定は？・重くない？ |
| 中に入る・渡す | exec -it ／ cp ／ -v | 入る・コピーする・共有する |
| イメージの一生 | pull → images → build → rmi | 取る・確認する・作る・消す |
| まとめて動かす | compose up → ps → logs → down | 起こす・見る・聞く・片付ける |
| お掃除 | system df → prune | まず量を見る → 次に消す（順番が大事） |

## 3. 状態確認（「今どうなってる？」を調べる）

Dockerで困ったら、**いきなり直そうとせず、まず状態を見る** のが基本です。

### 3-1. 診断フロー（困ったらこの順番）

| STEP | 見ること | コマンド |
|---|---|---|
| STEP1 | Dockerは生きている？ | `docker version` |
| STEP2 | コンテナは存在する？ 動いている？ | `docker ps -a` |
| STEP3 | Exited なら終了コードは？ | `docker inspect -f "{{.State.ExitCode}}" 名前` |
| STEP4 | 何とメッセージを出している？ | `docker logs --tail 50 名前` |
| STEP5 | ポート・マウントは意図どおり？ | `docker port 名前 ／ docker inspect -f "{{json .Mounts}}" 名前` |
| STEP6 | 中に入って確かめる | `docker exec -it 名前 sh` |
| STEP7 | 外から届く？ | `curl -I http://localhost:8080（PowerShellは curl.exe）` |

### 3-2. 状態確認コマンド一覧

> 💡 `--format` の `{{ }}` は、bash・PowerShell どちらでも **ダブルクォート "" で囲む** と動きます。`web` の部分は自分のコンテナ名に置き換えてください。

| 確認したいこと | コマンド | 見るポイント | 補足 |
|---|---|---|---|
| Dockerは動いている？ | `docker version` | Server: の欄に情報があればOK。無い・エラーなら未起動 | Docker Desktopのクジラアイコンが安定するまで待つ |
| Dockerの全体状況 | `docker info` | Containers（Running/Stopped）、Images、Server Version | トラブル相談時はこの出力を貼ると早い |
| 動いているコンテナ | `docker ps` | STATUS が Up か／PORTS に 8080->80 のような対応があるか | 何も出なければ動いているものはゼロ |
| 止まっているものも含め全部 | `docker ps -a` | Exited (数字) の数字が原因のヒント | 消し忘れの確認にも |
| 名前で絞り込む | `docker ps -a --filter "name=web"` | web を含む名前だけ表示 | 複数あるときに便利 |
| 止まったものだけ | `docker ps -a --filter "status=exited"` | Exitedのコンテナだけ表示 | 状態：created/running/paused/exited など |
| IDだけ一覧 | `docker ps -aq` | IDだけが並ぶ | 一括操作の材料（消す前に必ず中身を確認） |
| 見やすく整形 | `docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"` | 名前・状態・ポートだけの表 | 列が多くて見づらい時に |
| 状態を1語で | `docker inspect -f "{{.State.Status}}" web` | running / exited / created など | スクリプトで判定する時にも使える |
| 終了コード | `docker inspect -f "{{.State.ExitCode}}" web` | 0=正常 / 1=アプリのエラー / 127=コマンドが無い / 137=強制終了・メモリ不足 | 詳しくは「STATUSの読み方」表 |
| 再起動した回数 | `docker inspect -f "{{.RestartCount}}" web` | 増え続けていれば起動失敗を繰り返している | --restart 設定時に有効 |
| コンテナのIPアドレス | `docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" web` | IPアドレスが表示される | コンテナ同士はIPよりコンテナ名で繋ぐのが基本 |
| 公開ポートの対応 | `docker port web` | 80/tcp -> 0.0.0.0:8080 のように表示 | ブラウザで開けない時の確認 |
| 環境変数の確認 | `docker exec web env` | 渡したはずの -e が入っているか | 動作中のコンテナのみ |
| マウント設定の確認 | `docker inspect -f "{{json .Mounts}}" web` | Source（ホスト側）と Destination（コンテナ側） | フォルダが見えない時はまずここ |
| ログ（全体） | `docker logs web` | エラーメッセージ。最後の行から読む | まずこれ |
| ログ（末尾だけ） | `docker logs --tail 50 web` | 直近50行 | 長いログの時 |
| ログ（時間指定） | `docker logs --since 10m web` | 直近10分 | 10s / 10m / 2h などが使える |
| ログ（追跡） | `docker logs -f web` | リアルタイムに流れる。Ctrl+C で抜ける | 別ターミナルで操作しながら見ると効果的 |
| ログ（時刻つき） | `docker logs -t web` | 各行の先頭に時刻 | いつ起きたか調べたい時 |
| CPU・メモリ | `docker stats --no-stream` | CPU % と MEM USAGE / LIMIT | --no-stream なしだと連続表示（Ctrl+Cで終了） |
| コンテナ内のプロセス | `docker top web` | 動いているプロセス一覧 | アプリが本当に動いているか |
| ファイルの変更点 | `docker diff web` | A=追加 / C=変更 / D=削除 | コンテナ内で何が変わったか |
| ヘルスチェック | `docker inspect -f "{{.State.Health.Status}}" web` | healthy / unhealthy / starting | HEALTHCHECK を設定したコンテナのみ |
| イメージ一覧 | `docker images` | REPOSITORY / TAG / SIZE | -a で中間イメージも表示 |
| 容量の全体像 | `docker system df` | Images / Containers / Volumes の使用量 | 容量不足を感じたらまずこれ |
| 容量の内訳（詳細） | `docker system df -v` | どのイメージ・ボリュームが大きいか | 大きいものから整理 |
| ボリューム一覧 | `docker volume ls` | 名前の一覧 | DBデータが残っているか |
| ボリュームを使っているコンテナ | `docker ps -a --filter "volume=mydata"` | 対象ボリュームを使うコンテナ | 消す前の確認に |
| ネットワーク確認 | `docker network inspect mynet` | Containers の欄に参加者とIP | 繋がらない時の確認 |
| 出来事をリアルタイム表示 | `docker events` | start / stop / die などが流れる | Ctrl+C で抜ける。調査用 |
| Composeの状態 | `docker compose ps -a` | 各サービスの State と Ports | compose.yaml のあるフォルダで実行 |
| Composeの最終設定 | `docker compose config` | 変数展開後のYAML／文法エラー検出 | インデントの間違い発見に |
| コンテナの中から疎通確認 | `docker exec web curl -I http://localhost` | HTTP/1.1 200 OK など | curlが無いイメージも多い（その時は別途導入） |
| ホストから疎通確認（bash） | `curl -I http://localhost:8080` | HTTP/1.1 200 OK など | Ubuntu／VSCode（WSL）で実行 |
| ホストから疎通確認（PowerShell） | `curl.exe -I http://localhost:8080` | HTTP/1.1 200 OK など | PowerShellの curl は別物なので curl.exe と書く |
| ポートが開いているか（PowerShell） | `Test-NetConnection localhost -Port 8080` | TcpTestSucceeded : True なら到達可能 | PowerShell専用コマンド |

### 3-3. STATUS列の読み方（docker ps -a）

| STATUS表示 | 意味 | 次の一手 |
|---|---|---|
| Up 5 minutes | 動作中（5分経過） | PORTS・logs を確認して正常か見る |
| Up (healthy) / (unhealthy) | ヘルスチェックの結果 | unhealthy は docker logs で原因を探す |
| Exited (0) | 正常終了（仕事が終わった） | 常駐させたいなら、前面で動き続ける起動コマンドか確認 |
| Exited (1) | アプリがエラー終了 | docker logs 名前 の最後の行を読む |
| Exited (125) | docker run のオプション自体が誤り | オプションの綴り・順序を確認 |
| Exited (126) | 実行権限がない | ファイルの実行権限（chmod +x）を確認 |
| Exited (127) | コマンドが見つからない | CMD／ENTRYPOINT の綴り、イメージに含まれるか確認 |
| Exited (137) | 強制終了（kill／メモリ不足） | docker stats、メモリ上限、Docker Desktopのメモリ設定を確認 |
| Exited (143) | docker stop による通常の停止 | 通常は問題なし |
| Created | 作成されたが未起動 | docker start 名前 ／ 起動エラーは docker logs |
| Restarting | 起動に失敗して再起動を繰り返している | docker logs で原因確認、--restart の設定見直し |
| Paused | 一時停止中 | docker unpause 名前 |
| Dead | 削除に失敗した状態 | docker rm -f 名前 ／ Dockerを再起動 |

## 4. 環境別の違い（Ubuntu／VSCode／PowerShell／CMD）

| 項目 | Ubuntu（WSL）ターミナル | VSCode（WSL接続） | PowerShell | ポイント |
|---|---|---|---|---|
| 起動のしかた | スタートメニューから「Ubuntu」、またはWindows Terminalのタブ | Ubuntu内で code . → VSCode左下が「WSL: Ubuntu」になればOK。ターミナルは Ctrl+@（または Ctrl+&#96;） | スタートメニューから「PowerShell」またはWindows Terminal | VSCodeは「WSL」拡張機能が必要。左下の緑色の表示を毎回確認 |
| シェルの種類 | bash | WSL接続中はbash（PowerShellを選ぶことも可） | PowerShell | プロンプトの見た目で今どこにいるか判断する習慣を |
| コマンドの途中改行 | 行末に \（バックスラッシュ） | bash時は \ ／ PowerShell時は &#96; | 行末に &#96;（バッククォート） | 一番多いつまずき。コピペ前に環境を確認 |
| 現在のフォルダ指定 | $(pwd) または $PWD | bash時は $(pwd) | ${PWD}（例："${PWD}:/app"） | -v のときに使う。パスにスペースがあれば "" で囲む |
| 環境変数の設定 | export NAME=value | bash時は export | $env:NAME="value" | docker run -e とは別物（-eはコンテナ内の変数） |
| 画面クリア | clear または Ctrl+L | 同左 | cls または Clear-Host |  |
| 貼り付け | Ctrl+Shift+V（または右クリック） | Ctrl+V | Ctrl+V（または右クリック） | Ubuntu端末のCtrl+Vは効かないことがある |
| 処理の中断 | Ctrl+C | Ctrl+C | Ctrl+C | logs -f や stats を抜ける時に使用 |
| sudo | 通常は不要（dockerグループ所属時） | 同左 | 不要（sudoという概念がない） | 毎回 sudo が必要なら対処表を参照 |
| ファイルの置き場所 | ~/projects など Linux側を推奨 | 同左（WSL接続で開く） | Windows側 C:\Users\... でも可。ただし遅くなりやすい | WSL側→ /mnt/c/ 経由は遅い。作業は ~/ 配下が高速 |
| 相手側ファイルの見方 | Windowsの C: は /mnt/c/ | エクスプローラーで開くなら explorer.exe . | Ubuntuの中身は \\wsl.localhost\Ubuntu\home\ユーザー名 | エクスプローラーのアドレス欄に貼ると見られる |
| Dockerの実体 | Docker DesktopのWSL統合、またはUbuntu内に直接インストール | Ubuntu側のdockerを使用 | Docker Desktop付属のdocker.exe | どれも同じDockerエンジンに繋がる構成が一般的 |
| 便利拡張 | — | Docker拡張、Dev Containers、YAML | — | VSCodeのDockerアイコンからコンテナ一覧・ログ・シェル接続がクリックで可能 |

### 4-1. 同じ意味でも書き方が違う代表例

**Ubuntu / VSCode（bash）**
```bash
docker run -d \
  --name web \
  -p 8080:80 \
  -v "$(pwd)":/usr/share/nginx/html:ro \
  nginx:1.27
```

**PowerShell**
```powershell
docker run -d `
  --name web `
  -p 8080:80 `
  -v "${PWD}:/usr/share/nginx/html:ro" `
  nginx:1.27
```

**CMD（コマンドプロンプト）**
```bat
docker run -d ^
  --name web ^
  -p 8080:80 ^
  -v "%cd%:/usr/share/nginx/html:ro" ^
  nginx:1.27
```

### 4-2. WSL操作コマンド

| 操作 | コマンド | 説明 | 実行場所 |
|---|---|---|---|
| WSLのインストール | `wsl --install` | WSLとUbuntuを導入（管理者PowerShell）。完了後に再起動 | PowerShell(管理者) |
| ディストリ一覧と状態 | `wsl -l -v` | インストール済み一覧とVERSION(2が望ましい)、Running/Stopped | PowerShell |
| Ubuntuに入る | `wsl -d Ubuntu` | PowerShellからUbuntuのbashに入る。exitで戻る | PowerShell |
| WSLを完全停止 | `wsl --shutdown` | 設定変更や不具合時に再起動する前の定番 | PowerShell |
| WSLの更新 | `wsl --update` | WSL本体を最新にする | PowerShell |
| 既定の版を2に | `wsl --set-default-version 2` | 新規ディストリをWSL2で作る設定 | PowerShell |
| Ubuntuの更新 | `sudo apt update && sudo apt upgrade -y` | パッケージを最新に（最初に実施） | Ubuntu |
| VSCodeを開く | `code .` | 現在のフォルダをVSCode(WSL接続)で開く | Ubuntu |
| Windowsのエクスプローラーで開く | `explorer.exe .` | 現在のフォルダをエクスプローラーで表示 | Ubuntu |
| Docker Engine起動（直接導入の場合） | `sudo service docker start` | Docker Desktopを使わずUbuntu内に導入した場合に必要 | Ubuntu |
| dockerグループに参加 | `sudo usermod -aG docker $USER` | sudo無しでdockerを使えるように。実行後WSL再起動 | Ubuntu |
| インストール可能な一覧 | `wsl --list --online` | 導入できるディストリビューションの一覧（wsl -l -o） | PowerShell |
| 特定のUbuntuを導入 | `wsl --install -d Ubuntu-24.04` | 名前は上の一覧で確認 | PowerShell(管理者) |
| WSLのバージョン確認 | `wsl --version` | WSL本体のバージョン | PowerShell |
| 状態の確認 | `wsl --status` | 既定のディストリ・既定のバージョン等 | PowerShell |
| WSL1→2に変換 | `wsl --set-version Ubuntu 2` | Docker利用はWSL2が前提 | PowerShell |
| 既定のディストリを変更 | `wsl --set-default Ubuntu` | wsl だけで入る時の対象 | PowerShell |
| 特定のディストリだけ停止 | `wsl --terminate Ubuntu` | wsl -t Ubuntu でも可。全停止は --shutdown | PowerShell |
| rootで入る | `wsl -d Ubuntu -u root` | パスワード忘れ時の復旧にも | PowerShell |
| ホームフォルダで開く | `wsl ~` | 現在のWindowsフォルダでなくUbuntuのホームで開始 | PowerShell |
| Windowsのフォルダから開く | `wsl --cd C:\Users` | 指定フォルダで開始（/mnt/c/Users） | PowerShell |
| Ubuntuのコマンドを1回だけ実行 | `wsl docker ps` | PowerShellからUbuntuのコマンドを直接実行 | PowerShell |
| バックアップ（⭐重要） | `wsl --export Ubuntu D:\backup\ubuntu.tar` | Ubuntu全体をtarに保存。環境を壊す前に | PowerShell |
| バックアップから復元 | `wsl --import Ubuntu-restore D:\wsl\restore D:\backup\ubuntu.tar` | 別名で復元 | PowerShell |
| Ubuntuの削除（⚠） | `wsl --unregister Ubuntu` | ⚠ Ubuntu内のデータが全て消える。export後に | PowerShell |
| パス変換（Windows→WSL） | `wslpath 'C:\Users\name'` | → /mnt/c/Users/name | Ubuntu |
| パス変換（WSL→Windows） | `wslpath -w .` | → \\wsl.localhost\Ubuntu\home\... 形式 | Ubuntu |
| WindowsのコマンドをUbuntuから実行 | `powershell.exe -c Get-Date` | cmd.exe /c dir なども可 | Ubuntu |
| Windowsのブラウザで開く | `explorer.exe http://localhost:8080` | 既定のブラウザが開く | Ubuntu |
| WindowsのPATHの確認 | `echo $PATH` | /mnt/c/... が混ざっていると遅くなる場合あり | Ubuntu |
| ディスク使用量の確認 | `du -sh ~` | Ubuntu側の使用量。Docker利用時は増えやすい | Ubuntu |

### 4-3. WSLの設定ファイル（メモリ・systemd）

| ファイル | 場所 | 説明 |
|---|---|---|
| .wslconfig | Windows：C:\Users\ユーザー名\.wslconfig | WSL2全体（Docker Desktopも含む）のメモリ・CPU上限。メモリ不足や重い時に調整。反映は wsl --shutdown のあと |
| wsl.conf | Ubuntu：/etc/wsl.conf（sudoで編集） | systemdの有効化・既定ユーザー。反映は wsl --shutdown のあと |
| 確認コマンド | PowerShell／Ubuntu | ファイルが無ければ新規作成してよい |

**.wslconfig**（Windows：C:\Users\ユーザー名\.wslconfig）

```ini
[wsl2]
memory=6GB
processors=4
swap=2GB
```

**wsl.conf**（Ubuntu：/etc/wsl.conf（sudoで編集））

```ini
[boot]
systemd=true
[user]
default=ユーザー名
```

## 5. コマンド早見表

難易度：★=最初に覚える ／ ★★=慣れてきたら ／ ★★★=必要になったら

### 5-1. 確認・ヘルプ

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker --version` | Dockerのバージョンを表示 | `docker --version` | 最初に打つコマンド。表示されればインストールOK | ★ |
| `docker version` | クライアントとサーバー(エンジン)両方の情報 | `docker version` | Server欄が出ない・エラーなら Docker が起動していない | ★ |
| `docker info` | 環境情報（コンテナ数・イメージ数など） | `docker info` | 困ったとき、この出力を貼ると原因調査が早い | ★ |
| `docker --help` | コマンド一覧を表示 | `docker run --help` | 各コマンドの詳しい使い方は「docker コマンド名 --help」 | ★ |
| `docker run hello-world` | 動作確認用コンテナを実行 | `docker run hello-world` | 「Hello from Docker!」と出れば成功。初回はイメージも自動ダウンロード | ★ |
| `docker compose version` | Composeのバージョン確認 | `docker compose version` | ハイフン付きの docker-compose は旧版。新しい手順は docker compose | ★ |
| `docker buildx version` | Buildx（高機能ビルダー）のバージョン | `docker buildx version` | 複数CPU向けビルドで使う | ★★ |
| `docker context ls` | 接続先（Docker Desktop／リモート等）の一覧 | `docker context ls` | 接続先が違うと「コンテナが見えない」原因になる | ★★ |
| `docker context use` | 接続先を切り替える | `docker context use default` | *が付いているものが現在の接続先 | ★★★ |
| `docker init` | Dockerfile／compose.yaml の雛形を対話で生成 | `docker init` | 比較的新しい機能。バージョンで使えない場合あり | ★★ |

### 5-2. イメージ操作

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker pull` | イメージをダウンロード | `docker pull nginx:1.27` | タグ省略は latest。バージョンを固定すると再現性が上がる | ★ |
| `docker images` | 手元のイメージ一覧（docker image ls と同じ） | `docker images` | REPOSITORY=名前 / TAG=版 / SIZE=容量 | ★ |
| `docker search` | Docker Hubでイメージを検索 | `docker search ubuntu` | OFFICIAL（公式）マークのものを選ぶと安心 | ★ |
| `docker build -t` | Dockerfileからイメージを作る | `docker build -t myapp:1.0 .` | 最後の「.」は Dockerfile のあるフォルダ。忘れがち！ | ★★ |
| `docker tag` | イメージに別名（タグ）を付ける | `docker tag myapp:1.0 myapp:latest` | push前にユーザー名を付ける時にも使う | ★★ |
| `docker rmi` | イメージを削除 | `docker rmi nginx:1.27` | そのイメージを使うコンテナが残っていると消せない | ★ |
| `docker image inspect` | イメージの詳細情報（JSON） | `docker image inspect nginx:1.27` | 長いので --format や less と組み合わせる | ★★★ |
| `docker history` | イメージの作成履歴（レイヤー） | `docker history nginx:1.27` | どの命令で容量が増えたか確認できる | ★★★ |
| `docker image prune` | 使っていない（dangling）イメージを削除 | `docker image prune` | 実行前に確認メッセージが出る。-a で未使用すべて | ★★ |
| `docker images -a` | 中間イメージも含めて表示 | `docker images -a` |  | ★★ |
| `docker images -q` | イメージIDだけ表示 | `docker images -q` | 一括操作の材料。削除前に必ず中身を確認 | ★★ |
| `docker images --filter` | 条件で絞り込み | `docker images --filter "dangling=true"` | dangling=名前の無い不要イメージ | ★★ |
| `docker images --format` | 表示形式を指定 | `docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"` | 見やすい表にできる | ★★ |
| `docker pull --platform` | CPUの種類を指定して取得 | `docker pull --platform linux/amd64 nginx:1.27` | Apple Silicon等で互換性問題がある時 | ★★★ |
| `docker build -f` | Dockerfileのファイル名を指定 | `docker build -f Dockerfile.dev -t myapp:dev .` | 複数のDockerfileを使い分ける | ★★ |
| `docker build --no-cache` | キャッシュを使わず作り直す | `docker build --no-cache -t myapp:1.0 .` | 古い内容が残っている疑いのある時 | ★★ |
| `docker build --build-arg` | ビルド時の変数を渡す | `docker build --build-arg NODE_ENV=production -t myapp .` | Dockerfile側で ARG の宣言が必要 | ★★★ |
| `docker build --target` | マルチステージの途中まで作る | `docker build --target build -t myapp:build .` | FROM ... AS 名前 の名前を指定 | ★★★ |
| `docker build --platform` | 対象CPUを指定してビルド | `docker build --platform linux/amd64 -t myapp .` |  | ★★★ |
| `docker buildx build` | 複数CPU向けなどの高機能ビルド | `docker buildx build --platform linux/amd64,linux/arm64 -t user/myapp:1.0 --push .` | push先（レジストリ）が必要 | ★★★ |
| `docker commit` | コンテナの状態からイメージを作る | `docker commit web myimg:1.0` | 再現性が低いので学習・一時用。基本は Dockerfile | ★★★ |
| `docker image prune -a` | 使っていないイメージをすべて削除 | `docker image prune -a` | ⚠ 再取得が必要になる。内容を読んでから y | ★★ |

### 5-3. コンテナ操作

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker run` | イメージからコンテナを作って起動 | `docker run -d --name web -p 8080:80 nginx:1.27` | pull も自動。オプションは次の表を参照 | ★ |
| `docker ps` | 動いているコンテナ一覧 | `docker ps` | PORTS列で公開ポート、STATUS列で状態を確認 | ★ |
| `docker ps -a` | 停止中も含めた全コンテナ一覧 | `docker ps -a` | Exited は停止中。消し忘れの確認に使う | ★ |
| `docker stop` | コンテナを停止（やさしく終了） | `docker stop web` | 名前でもIDの先頭数文字でも指定OK | ★ |
| `docker start` | 停止中のコンテナを再起動 | `docker start web` | run は「新規作成」、start は「既存を起動」 | ★ |
| `docker restart` | コンテナを再起動 | `docker restart web` | 設定ファイル変更後などに使う | ★ |
| `docker rm` | コンテナを削除 | `docker rm web` | 停止中のみ削除可。動作中は -f で強制 | ★ |
| `docker logs` | コンテナのログを表示 | `docker logs -f --tail 50 web` | -f=追跡表示 / --tail 50=末尾50行。Ctrl+C で抜ける | ★ |
| `docker exec -it` | 動作中のコンテナ内でコマンド実行 | `docker exec -it web bash` | 中に入る定番。抜けるときは exit。bashが無ければ sh | ★★ |
| `docker cp` | ホストとコンテナ間でファイルコピー | `docker cp ./index.html web:/usr/share/nginx/html/` | コピー元:コピー先 の順。コンテナ側は「名前:パス」 | ★★ |
| `docker inspect` | コンテナの詳細情報（IP・マウント等） | `docker inspect web` | JSONで出る。IPやマウント設定の確認に便利 | ★★★ |
| `docker stats` | CPU・メモリ使用量をリアルタイム表示 | `docker stats` | Ctrl+C で終了。重い原因調査に | ★★ |
| `docker top` | コンテナ内のプロセス一覧 | `docker top web` | コンテナの中で何が動いているか | ★★ |
| `docker rename` | コンテナ名を変更 | `docker rename web web-old` | 名前の付け間違い時に | ★ |
| `docker kill` | コンテナを強制停止 | `docker kill web` | stop で止まらない時の最終手段 | ★★ |
| `docker container prune` | 停止中コンテナをまとめて削除 | `docker container prune` | 確認メッセージあり。消す前に docker ps -a で確認 | ★★ |
| `docker create` | 作成だけして起動しない | `docker create --name web nginx:1.27` | docker start 名前 で起動 | ★★ |
| `docker ps -s` | サイズも表示 | `docker ps -s` | コンテナが書き込んだ容量が分かる | ★★ |
| `docker ps --no-trunc` | 省略せず全部表示 | `docker ps --no-trunc` | 長い COMMAND や ID を全部見る | ★★ |
| `docker ps -n 3` | 直近N件だけ表示 | `docker ps -a -n 3` | 最後に作ったコンテナの確認 | ★★ |
| `docker ps -l` | 最後に作ったコンテナ1件 | `docker ps -l` |  | ★★ |
| `docker stop -t` | 停止の待ち秒数を指定 | `docker stop -t 30 web` | 既定は10秒。終了処理が長いアプリ向け | ★★ |
| `docker stop（全部）` | 動いているコンテナを全部止める | `docker stop $(docker ps -q)` | PowerShell: docker ps -q \| % { docker stop $_ } | ★★ |
| `docker rm -v` | 関連する匿名ボリュームも削除 | `docker rm -v web` |  | ★★★ |
| `docker pause／unpause` | 一時停止／再開 | `docker pause web` | プロセスを凍結する。stopとは別 | ★★★ |
| `docker wait` | 終了を待って終了コードを返す | `docker wait web` | スクリプト向け | ★★★ |
| `docker update` | 動作中の設定を変更 | `docker update --restart unless-stopped web` | 再起動方針・CPU・メモリの上限など | ★★★ |
| `docker attach` | コンテナの標準入出力に接続 | `docker attach web` | ⚠ Ctrl+C で止まることがある。切り離しは Ctrl+P → Ctrl+Q | ★★★ |
| `docker exec -u` | ユーザーを指定して実行 | `docker exec -u root -it web bash` | 権限不足の確認に | ★★ |
| `docker exec -w` | 作業ディレクトリを指定して実行 | `docker exec -w /app web ls` |  | ★★ |
| `docker exec -e` | 環境変数を渡して実行 | `docker exec -e DEBUG=1 web env` |  | ★★ |
| `docker exec -d` | バックグラウンドで実行 | `docker exec -d web touch /tmp/a` |  | ★★★ |
| `docker run --rm イメージ コマンド` | 使い捨てで1コマンドだけ実行 | `docker run --rm node:22 node -v` | ツールをインストールせずに試せる | ★ |
| `docker diff` | ファイルの変更点を表示 | `docker diff web` | A=追加 C=変更 D=削除 | ★★ |
| `docker port` | 公開ポートの対応を表示 | `docker port web` |  | ★ |
| `docker events` | 出来事（起動・停止等）をリアルタイム表示 | `docker events --since 10m` | Ctrl+C で抜ける | ★★★ |

### 5-4. docker run のオプション

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `-d` | バックグラウンドで起動 | `docker run -d nginx:1.27` | 付けないと画面を占有する（Ctrl+Cで停止） | ★ |
| `-it` | 対話モード（キーボード入力できる） | `docker run -it ubuntu:24.04 bash` | -i=入力 / -t=端末。シェルに入る時はセット | ★ |
| `--rm` | 終了時にコンテナを自動削除 | `docker run --rm -it ubuntu:24.04 bash` | お試し実行で残骸を作らない。新人にオススメ | ★ |
| `--name` | コンテナに名前を付ける | `docker run -d --name web nginx:1.27` | 同名は作れない（Conflictエラー） | ★ |
| `-p ホスト:コンテナ` | ポートを公開 | `docker run -d -p 8080:80 nginx:1.27` | 左=自分のPC、右=コンテナ内。http://localhost:8080 で見える | ★ |
| `-v ホスト:コンテナ` | フォルダ/ボリュームをマウント | `docker run -v "$(pwd)":/app ...` | PowerShellは "${PWD}:/app"。:ro で読み取り専用 | ★★ |
| `-e KEY=VALUE` | 環境変数を渡す | `docker run -e MYSQL_ROOT_PASSWORD=pass mysql:8` | パスワード直書きは学習用のみ。本番は秘匿管理 | ★★ |
| `--network` | 接続するネットワークを指定 | `docker run --network mynet ...` | 同じネットワーク内ならコンテナ名で通信できる | ★★ |
| `--restart` | 自動再起動の方針 | `docker run -d --restart unless-stopped ...` | no / on-failure / always / unless-stopped | ★★ |
| `-w` | 作業ディレクトリを指定 | `docker run -w /app ...` | コンテナ内の最初のカレントフォルダ | ★★ |
| `--env-file` | ファイルから環境変数を読む | `docker run --env-file .env nginx` | パスワードの直書きを減らせる。.env はGitに入れない | ★★ |
| `--user` | 実行ユーザーを指定 | `docker run --user 1000:1000 ...` | rootで動かさない方が安全 | ★★★ |
| `--entrypoint` | 起動コマンドを上書き | `docker run --rm -it --entrypoint sh nginx:1.27` | 中身を調べたい時の定番 | ★★ |
| `-m ／ --memory` | メモリ上限 | `docker run -m 512m ...` | 超えると Exited (137) の原因になる | ★★ |
| `--cpus` | CPU上限 | `docker run --cpus 1.5 ...` |  | ★★★ |
| `--hostname` | コンテナ内のホスト名 | `docker run --hostname web01 ...` |  | ★★★ |
| `--add-host` | hostsに名前を追加 | `docker run --add-host host.docker.internal:host-gateway ...` | コンテナからホストを参照したい時 | ★★★ |
| `--read-only` | ファイルシステムを読み取り専用に | `docker run --read-only --tmpfs /tmp ...` | 安全性向上 | ★★★ |
| `--tmpfs` | メモリ上の一時領域 | `docker run --tmpfs /tmp ...` |  | ★★★ |
| `--platform` | CPUの種類を指定 | `docker run --platform linux/amd64 ...` |  | ★★★ |
| `--init` | プロセス管理を補助 | `docker run --init ...` | ゾンビプロセスの回収 | ★★★ |
| `--pull always` | 起動前に必ず最新を取得 | `docker run --pull always nginx` |  | ★★★ |
| `--health-cmd` | ヘルスチェックを指定 | `docker run --health-cmd "curl -f http://localhost/" --health-interval 10s ...` | 結果は docker ps の STATUS（healthy）に出る | ★★★ |
| `-l ／ --label` | ラベル（メモ）を付ける | `docker run -l env=dev ...` | ps --filter "label=env=dev" で絞り込める | ★★★ |
| `--mount` | マウントを詳しく指定 | `docker run --mount type=bind,src="$(pwd)",dst=/app nginx` | -v より誤りに気付きやすい（無いパスはエラー） | ★★★ |
| `-v 名前:パス` | 名前付きボリューム | `docker run -v mydata:/var/lib/mysql mysql:8` | DBデータを残す定番 | ★★ |
| `-v パスのみ` | 匿名ボリューム | `docker run -v /app/node_modules ...` | bind mount でホスト側に上書きされたくない場所に | ★★★ |
| `-p 127.0.0.1:8080:80` | 自分のPCからだけ公開 | `docker run -p 127.0.0.1:8080:80 nginx` | 他のPCからアクセスさせたくない時 | ★★ |
| `-P` | ポートを自動割り当てで公開 | `docker run -d -P nginx` | docker port 名前 で確認 | ★★★ |
| `--network host` | ホストのネットワークをそのまま使う | `docker run --network host ...` | Docker Desktop（Windows）では挙動が異なる場合あり | ★★★ |
| `--log-opt` | ログのサイズ上限 | `docker run --log-opt max-size=10m --log-opt max-file=3 ...` | ログでディスクが埋まるのを防ぐ | ★★★ |
| `--privileged` | ほぼ全権限を与える | `docker run --privileged ...` | ⚠ 危険。理由が分からない場合は使わない | ★★★ |

### 5-5. ボリューム（データ保存）

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker volume create` | ボリュームを作成 | `docker volume create mydata` | データをコンテナ削除後も残したい時（DBなど） | ★★ |
| `docker volume ls` | ボリューム一覧 | `docker volume ls` |  | ★ |
| `docker volume inspect` | ボリューム詳細 | `docker volume inspect mydata` | 保存場所(Mountpoint)などが分かる | ★★ |
| `docker volume rm` | ボリューム削除 | `docker volume rm mydata` | 中のデータも消える！慎重に | ★★ |
| `docker volume prune` | 未使用ボリュームを一括削除 | `docker volume prune` | データ消失に注意。実行前に必ず ls で確認 | ★★★ |
| `docker volume ls -f dangling=true` | 使われていないボリューム一覧 | `docker volume ls -f dangling=true` | prune の前に必ず確認 | ★★ |
| `バックアップ（tar）` | ボリュームの中身をファイルに保存 | `docker run --rm -v mydata:/data -v "$(pwd)":/backup ubuntu tar czf /backup/mydata.tgz -C /data .` | PowerShellは "${PWD}:/backup" | ★★★ |
| `復元（tar）` | バックアップからボリュームへ戻す | `docker run --rm -v mydata:/data -v "$(pwd)":/backup ubuntu tar xzf /backup/mydata.tgz -C /data` | 既存データが上書きされるので注意 | ★★★ |

### 5-6. ネットワーク

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker network ls` | ネットワーク一覧 | `docker network ls` | bridge / host / none が標準 | ★ |
| `docker network create` | ネットワーク作成 | `docker network create mynet` | 自作ネットワーク内はコンテナ名で名前解決できる | ★★ |
| `docker network inspect` | ネットワーク詳細 | `docker network inspect mynet` | 参加コンテナとIPが分かる | ★★ |
| `docker network connect` | 動作中コンテナを接続 | `docker network connect mynet web` | 切断は disconnect | ★★★ |
| `docker network rm` | ネットワーク削除 | `docker network rm mynet` | 使用中のコンテナがあると消せない | ★★ |
| `docker network create --subnet` | IP範囲を指定して作成 | `docker network create --subnet 172.20.0.0/16 mynet` | 会社のネットワークと被る時に | ★★★ |
| `docker network disconnect` | ネットワークから切り離す | `docker network disconnect mynet web` |  | ★★★ |
| `docker network prune` | 未使用ネットワークを削除 | `docker network prune` | 確認メッセージあり | ★★ |
| `名前解決の確認` | コンテナから他のコンテナ名を引く | `docker exec web getent hosts db` | 同じネットワークにいるかの確認 | ★★★ |

### 5-7. Dockerfileの命令

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `FROM` | ベースイメージを指定 | `FROM node:22-slim` | 必ず最初に書く | ★ |
| `WORKDIR` | 作業ディレクトリ指定 | `WORKDIR /app` | 以降のCOPY/RUNの基準フォルダになる | ★ |
| `COPY` | ファイルをイメージにコピー | `COPY . .` | 左=ホスト（ビルド文脈）、右=イメージ内 | ★ |
| `RUN` | ビルド時にコマンド実行 | `RUN npm install` | 命令ごとにレイヤーが増える。&& でまとめると軽い | ★★ |
| `ENV` | 環境変数を設定 | `ENV NODE_ENV=production` |  | ★ |
| `EXPOSE` | 使うポートの目印（公開はしない） | `EXPOSE 3000` | 実際の公開は docker run -p で行う | ★ |
| `CMD` | コンテナ起動時の既定コマンド | `CMD ["node", "app.js"]` | docker run の末尾で上書き可能 | ★★ |
| `ENTRYPOINT` | 起動時に必ず実行するコマンド | `ENTRYPOINT ["python"]` | CMD は ENTRYPOINT への引数になる | ★★★ |
| `ARG` | ビルド時だけの変数 | `ARG VERSION=1.0` | ENVと違い実行時には残らない | ★★ |
| `USER` | 実行ユーザーを指定 | `USER node` | rootのままにしない | ★★ |
| `VOLUME` | ボリュームの場所を宣言 | `VOLUME /data` |  | ★★★ |
| `LABEL` | メタ情報を付ける | `LABEL maintainer="you@example.com"` |  | ★★★ |
| `HEALTHCHECK` | ヘルスチェックを定義 | `HEALTHCHECK CMD curl -f http://localhost/ \|\| exit 1` | docker ps の STATUS に healthy が出る | ★★★ |
| `ADD` | COPY＋URL取得・tar展開 | `ADD app.tar.gz /app/` | 基本は COPY を使う | ★★ |
| `FROM ... AS` | マルチステージビルド（段階名） | `FROM node:22 AS build` | 最終イメージを小さくできる | ★★★ |
| `COPY --from` | 別の段階から取り出す | `COPY --from=build /app/dist /usr/share/nginx/html` | ビルド用ツールを最終イメージに入れない | ★★★ |
| `RUN（apt軽量化）` | aptを使う時の定番書き方 | `RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/*` | 1つのRUNにまとめてキャッシュを残さない | ★★★ |

### 5-8. Docker Compose

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker compose up -d` | compose.yamlの内容を一括起動 | `docker compose up -d` | 複数コンテナをまとめて起動。-d でバックグラウンド | ★★ |
| `docker compose down` | 一括停止＋削除 | `docker compose down` | -v を付けるとボリュームも削除（データ消失注意） | ★★ |
| `docker compose ps` | Compose管理下のコンテナ一覧 | `docker compose ps` | compose.yaml のあるフォルダで実行 | ★★ |
| `docker compose logs -f` | ログを追跡表示 | `docker compose logs -f web` | サービス名を付けると絞り込み | ★★ |
| `docker compose build` | イメージを再ビルド | `docker compose build` | Dockerfile変更後。up --build でも可 | ★★ |
| `docker compose exec` | サービス内でコマンド実行 | `docker compose exec web bash` | docker exec のCompose版（コンテナ名不要） | ★★ |
| `docker compose stop / start` | 停止／再開（削除しない） | `docker compose stop` | down との違い：stopはコンテナを残す | ★★ |
| `docker compose config` | 設定ファイルの文法チェック＆展開表示 | `docker compose config` | YAMLのインデント間違い発見に便利 | ★★ |
| `docker compose up -d --build` | 再ビルドして起動 | `docker compose up -d --build` | Dockerfileを変更した時 | ★★ |
| `docker compose up --force-recreate` | コンテナを作り直す | `docker compose up -d --force-recreate` |  | ★★★ |
| `docker compose up -d サービス名` | 特定のサービスだけ起動 | `docker compose up -d web` | 依存するサービスも一緒に起動 | ★★ |
| `docker compose up --scale` | 台数を増やす | `docker compose up -d --scale web=3` | ホスト側ポートが固定だと衝突する | ★★★ |
| `docker compose -f` | 設定ファイルを指定（複数可） | `docker compose -f compose.yaml -f compose.dev.yaml up -d` | 後ろのファイルが上書きされる | ★★★ |
| `docker compose --profile` | プロファイルを指定 | `docker compose --profile debug up -d` | compose.yaml の profiles と組み合わせる | ★★★ |
| `docker compose run --rm` | 1回だけコマンドを実行 | `docker compose run --rm web sh` | DBのマイグレーション等に | ★★ |
| `docker compose pull` | イメージを最新に取得 | `docker compose pull` |  | ★★ |
| `docker compose push` | イメージをレジストリへ送る | `docker compose push` |  | ★★★ |
| `docker compose top` | 各サービスのプロセス | `docker compose top` |  | ★★★ |
| `docker compose images` | 使っているイメージ一覧 | `docker compose images` |  | ★★★ |
| `docker compose ls` | Composeプロジェクトの一覧 | `docker compose ls` | 別フォルダのプロジェクトも見える | ★★ |
| `docker compose port` | サービスの公開ポート確認 | `docker compose port web 80` |  | ★★★ |
| `docker compose cp` | コンテナとホスト間でコピー | `docker compose cp web:/etc/nginx/nginx.conf .` |  | ★★★ |
| `docker compose kill` | 強制停止 | `docker compose kill web` |  | ★★★ |
| `docker compose rm -f` | 停止中のコンテナを削除 | `docker compose rm -f` |  | ★★ |
| `docker compose pause／unpause` | 一時停止／再開 | `docker compose pause` |  | ★★★ |
| `docker compose restart` | 再起動 | `docker compose restart web` | 設定ファイルの反映には up -d が必要な場合も | ★★ |
| `docker compose down --rmi local` | 停止・削除＋ビルドしたイメージも削除 | `docker compose down --rmi local` |  | ★★★ |
| `docker compose down --remove-orphans` | 使われなくなったコンテナも削除 | `docker compose down --remove-orphans` | サービス名を変えた後の残骸掃除 | ★★★ |
| `docker compose config --services` | サービス名の一覧 | `docker compose config --services` |  | ★★ |
| `docker compose logs --tail` | ログの末尾だけ表示 | `docker compose logs --tail 100 -f web` |  | ★★ |
| `docker compose watch` | ファイル変更を検知して自動反映 | `docker compose watch` | 新しめの機能。compose.yaml の develop 設定が必要 | ★★★ |
| `docker compose events` | 出来事をリアルタイム表示 | `docker compose events` |  | ★★★ |
| `docker compose -p` | プロジェクト名を指定 | `docker compose -p myproj up -d` | 同じ構成を複数起動したい時 | ★★★ |
| `docker compose --env-file` | 環境変数ファイルを指定 | `docker compose --env-file .env.prod up -d` |  | ★★★ |
| `docker compose build --no-cache` | キャッシュ無しで再ビルド | `docker compose build --no-cache` |  | ★★ |
| `docker compose exec -T` | TTY無しで実行（スクリプト向け） | `docker compose exec -T db pg_dump -U user db > dump.sql` | パイプ・リダイレクトで文字化けしない | ★★★ |

### 5-9. お掃除・容量

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker system df` | Dockerの使用容量を表示 | `docker system df` | 容量不足を感じたら最初にこれ | ★ |
| `docker system prune` | 未使用のコンテナ・ネットワーク等を一括削除 | `docker system prune` | 確認あり。何が消えるか読んでから y | ★★ |
| `docker system prune -a --volumes` | 未使用イメージ・ボリュームまで全削除 | `docker system prune -a --volumes` | ⚠危険：データも消える。新人は単独で使わない | ★★★ |
| `docker builder prune` | ビルドキャッシュを削除 | `docker builder prune` | ビルドで容量が増えた時 | ★★ |
| `docker system prune --filter` | 古いものだけ削除 | `docker system prune --filter "until=24h"` | 24時間以上前のもの | ★★ |
| `docker system prune -f` | 確認なしで削除 | `docker system prune -f` | ⚠ 確認が出ないので、初心者は使わない | ★★★ |
| `停止済みコンテナを全部削除` | exitedのものだけ削除 | `docker rm $(docker ps -aq -f status=exited)` | PowerShell: docker ps -aq -f status=exited \| % { docker rm $_ } | ★★ |

### 5-10. レジストリ（Docker Hub）

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker login` | Docker Hub等にログイン | `docker login` | パスワードよりアクセストークン推奨 | ★★ |
| `docker push` | イメージをアップロード | `docker push ユーザー名/myapp:1.0` | 公開範囲に注意（社外秘を入れない） | ★★ |
| `docker logout` | ログアウト | `docker logout` | 共用PCでは必ず実行 | ★ |
| `docker login --password-stdin` | トークンを標準入力で渡してログイン | `docker login -u ユーザー名 --password-stdin` | パスワードをコマンド履歴に残さない | ★★ |
| `docker login ghcr.io` | GitHub Container Registry にログイン | `docker login ghcr.io -u ユーザー名` | GitHubのトークンを使用 | ★★★ |
| `docker push（GHCR）` | GitHub Container Registry へ送る | `docker push ghcr.io/ユーザー名/myapp:1.0` |  | ★★★ |
| `docker search --filter` | 公式のみ検索 | `docker search --filter is-official=true ubuntu` |  | ★★ |

### 5-11. 保存・移行・調査

| コマンド | 意味 | 使用例 | ポイント・注意 | 難易度 |
|---|---|---|---|---|
| `docker save` | イメージをファイルに保存 | `docker save -o myapp.tar myapp:1.0` | ネットに繋がらない環境への持ち込みに | ★★★ |
| `docker load` | 保存したイメージを読み込む | `docker load -i myapp.tar` |  | ★★★ |
| `docker export` | コンテナのファイル一式を書き出す | `docker export web -o web.tar` | イメージのレイヤー情報は含まれない | ★★★ |
| `docker import` | tarからイメージを作る | `docker import web.tar myimg:1.0` |  | ★★★ |
| `docker image inspect -f（公開ポート）` | イメージのEXPOSEを確認 | `docker image inspect -f "{{.Config.ExposedPorts}}" nginx:1.27` |  | ★★★ |
| `docker inspect -f（起動コマンド）` | CMDとENTRYPOINTを確認 | `docker inspect -f "{{.Config.Cmd}} {{.Config.Entrypoint}}" web` | 起動すぐ終了する時の確認 | ★★ |
| `docker inspect \| jq` | JSONを見やすく整形・抽出 | `docker inspect web \| jq '.[0].State'` | jq は apt install jq が必要 | ★★★ |
| `docker stats --format` | 統計を整形して表示 | `docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"` |  | ★★ |

## 6. Dockerfile ひな形（Node.jsの例）

```dockerfile
FROM node:22-slim
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

`.dockerignore`（ビルドに含めたくないもの）

```
node_modules
.git
*.log
.env
```

```bash
docker build -t myapp:1.0 .
docker run -d --name myapp -p 3000:3000 myapp:1.0
```

## 7. compose.yaml ひな形（Web + DB）

```yaml
services:
  web:
    image: nginx:1.27
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    restart: unless-stopped
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: pass   # 学習用。本番では秘匿管理に
    volumes:
      - dbdata:/var/lib/mysql
volumes:
  dbdata:
```

```bash
docker compose up -d     # 起動
docker compose ps        # 確認
docker compose logs -f   # ログ
docker compose down      # 停止＋削除（-v でボリュームも削除）
```

> ⚠ YAMLは **インデントはスペース2つ**、タブ文字は使えません。

### 7-1. compose.yaml のキー早見表

| キー | 意味 | 補足 |
|---|---|---|
| `services` | サービス（コンテナ）の定義の親 | 最上位のキー |
| `image` | 使うイメージ | タグを固定する |
| `build` | Dockerfileからビルド | imageと両方書くと、ビルド後の名前になる |
| `container_name` | コンテナ名を固定 | 固定すると同名で複数起動できなくなる |
| `ports` | ポート公開 | "" で囲む（YAMLの解釈ミス防止） |
| `expose` | コンテナ間だけに公開 | ホストからは見えない |
| `volumes` | マウント | 最後に volumes: で名前付きボリュームを宣言 |
| `environment` | 環境変数 | パスワードの直書きは学習用のみ |
| `env_file` | 環境変数ファイル | Gitに入れない |
| `depends_on` | 起動順の依存 | healthcheck と組み合わせると「準備完了を待つ」 |
| `healthcheck` | ヘルスチェック | STATUS に healthy が出る |
| `restart` | 再起動方針 | no／on-failure／always／unless-stopped |
| `networks` | 参加するネットワーク | 最上位の networks: で宣言 |
| `command` | 起動コマンドの上書き | Dockerfile の CMD を上書き |
| `entrypoint` | ENTRYPOINTの上書き |  |
| `working_dir` | 作業ディレクトリ |  |
| `user` | 実行ユーザー | rootで動かさない |
| `profiles` | 特定の時だけ起動 | --profile debug 指定時のみ起動 |
| `labels` | ラベル |  |
| `logging` | ログ設定 | ログ肥大の防止 |
| `extra_hosts` | hostsに追加 |  |
| `stdin_open／tty` | 対話用（-it 相当） | シェルを開きっぱなしにしたいコンテナ向け |
| `platform` | CPUの種類 |  |
| `tmpfs` | メモリ上の一時領域 |  |

**書き方の例（キーごと）**

```yaml
# services … サービス（コンテナ）の定義の親
services:
  web:

# image … 使うイメージ
image: nginx:1.27

# build … Dockerfileからビルド
build: .
（詳細）build:
  context: .
  dockerfile: Dockerfile.dev

# container_name … コンテナ名を固定
container_name: web

# ports … ポート公開
ports:
  - "8080:80"

# expose … コンテナ間だけに公開
expose:
  - "3000"

# volumes … マウント
volumes:
  - ./html:/usr/share/nginx/html:ro
  - dbdata:/var/lib/mysql

# environment … 環境変数
environment:
  TZ: Asia/Tokyo

# env_file … 環境変数ファイル
env_file:
  - .env

# depends_on … 起動順の依存
depends_on:
  db:
    condition: service_healthy

# healthcheck … ヘルスチェック
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 10s
  retries: 5

# restart … 再起動方針
restart: unless-stopped

# networks … 参加するネットワーク
networks:
  - backend

# command … 起動コマンドの上書き
command: ["npm", "run", "dev"]

# entrypoint … ENTRYPOINTの上書き
entrypoint: ["sh", "-c"]

# working_dir … 作業ディレクトリ
working_dir: /app

# user … 実行ユーザー
user: "1000:1000"

# profiles … 特定の時だけ起動
profiles: ["debug"]

# labels … ラベル
labels:
  env: dev

# logging … ログ設定
logging:
  options:
    max-size: "10m"
    max-file: "3"

# extra_hosts … hostsに追加
extra_hosts:
  - "host.docker.internal:host-gateway"

# stdin_open／tty … 対話用（-it 相当）
stdin_open: true
tty: true

# platform … CPUの種類
platform: linux/amd64

# tmpfs … メモリ上の一時領域
tmpfs:
  - /tmp

```

## 8. 絶対に覚えたい「危険コマンド」

| コマンド | 何が起きる？ | 対策 |
|---|---|---|
| `docker system prune -a --volumes` | 未使用のイメージ・ボリュームが全消去 | 実行前に docker system df と docker volume ls を確認 |
| `docker volume prune ／ docker volume rm` | 保存データが消える | DBのボリュームは特に注意 |
| `docker compose down -v` | ボリュームも消える | -v を付けるのは本当に消してよい時だけ |
| `docker rm -f` | 動作中でも強制削除 | まず docker stop を使う |
| `rm -rf（Ubuntu）／ Remove-Item -Recurse -Force（PowerShell）` | フォルダごと削除。戻せない | 実行前に ls ／ Get-ChildItem でパスを確認 |

## 9. 覚える順番のおすすめ（1週間プラン）

| 日 | 学ぶこと | コマンド |
|---|---|---|
| 1日目 | 動作確認・イメージ取得 | --version / run hello-world / pull / images |
| 2日目 | コンテナを動かす | run -d -p --name / ps / stop / rm |
| 3日目 | 中を見る・状態確認 | logs / exec -it / cp / inspect / stats |
| 4日目 | データを残す | -v / volume |
| 5日目 | 自分のイメージを作る | Dockerfile / build / tag |
| 6日目 | まとめて動かす | compose up / down / logs |
| 7日目 | お掃除と復習 | system df / prune |

## 10. 練習ハンズオン（上から順に実行）

| STEP | やること | Ubuntu / VSCode（bash） | PowerShell | 確認ポイント |
|---|---|---|---|---|
| 1 | バージョン確認 | `docker --version` | `docker --version` | バージョンが表示される |
| 2 | 動作確認 | `docker run --rm hello-world` | `docker run --rm hello-world` | 「Hello from Docker!」が表示 |
| 3 | イメージ取得 | `docker pull nginx:1.27 && docker images` | `docker pull nginx:1.27; docker images` | nginxがイメージ一覧に出る |
| 4 | Webサーバー起動 | `docker run -d --name web -p 8080:80 nginx:1.27` | `docker run -d --name web -p 8080:80 nginx:1.27` | ブラウザで http://localhost:8080 が開く |
| 5 | 状態確認 | `docker ps` | `docker ps` | STATUSがUp、PORTSに 8080->80 |
| 6 | ログ確認 | `docker logs web` | `docker logs web` | アクセスログが出ている |
| 7 | コンテナ内に入る | `docker exec -it web bash   （終わったら exit）` | `docker exec -it web bash   （終わったら exit）` | プロンプトが root@xxxx に変わる |
| 8 | フォルダ連携の準備 | `mkdir -p ~/docker-lab && cd ~/docker-lab && echo '<h1>Hello Docker</h1>' > index.html` | `mkdir docker-lab; cd docker-lab; '<h1>Hello Docker</h1>' \| Out-File index.html -Encoding utf8` | index.html ができる |
| 9 | 停止と削除 | `docker stop web && docker rm web` | `docker stop web; docker rm web` | docker ps -a にwebが無い |
| 10 | 自分のHTMLを表示 | `docker run -d --name web2 -p 8080:80 -v "$(pwd)":/usr/share/nginx/html:ro nginx:1.27` | `docker run -d --name web2 -p 8080:80 -v "${PWD}:/usr/share/nginx/html:ro" nginx:1.27` | ブラウザに「Hello Docker」が表示 |
| 11 | お片付け | `docker rm -f web2 && docker rmi nginx:1.27` | `docker rm -f web2; docker rmi nginx:1.27` | docker ps -a / docker images が空に近い |

## 11. トラブル対処

| 症状（エラーメッセージ） | 原因 | 対処 | 環境 |
|---|---|---|---|
| permission denied while trying to connect to the Docker daemon socket | ユーザーがdockerグループに入っていない | sudo usermod -aG docker $USER を実行 → PowerShellで wsl --shutdown → Ubuntuを開き直す | Ubuntu |
| Cannot connect to the Docker daemon at unix:///var/run/docker.sock | Dockerが起動していない／WSL統合がOFF | Docker Desktopを起動。Settings > Resources > WSL integration でUbuntuをON。直接導入なら sudo service docker start | Ubuntu/VSCode |
| docker: command not found | Dockerが未導入、またはWSL統合がOFF | Docker DesktopのWSL integrationをON → Ubuntuを開き直す | Ubuntu/VSCode |
| failed to connect to the docker API / error during connect（PowerShell） | Docker Desktopが起動していない | タスクトレイのクジラアイコンが安定するまで待つ。起動後に docker version で確認 | PowerShell |
| port is already allocated / address already in use | 同じホストポートを別のものが使用中 | docker ps で確認し、-p 8081:80 のように左側の番号を変える | 全環境 |
| Conflict. The container name "/web" is already in use | 同名の（停止中）コンテナが残っている | docker rm web で削除、または別の --name を使う | 全環境 |
| pull access denied / repository does not exist | イメージ名の綴り間違い、または非公開 | docker search や Docker Hubで名前確認。非公開なら docker login | 全環境 |
| コンテナがすぐ Exited になる | 前面で動き続けるプロセスがない／起動コマンドの失敗 | docker logs 名前 で原因確認。シェルは -it を付ける | 全環境 |
| localhost:8080 に繋がらない | -p 未指定、ポート違い、アプリが 127.0.0.1 でしか待っていない | docker ps のPORTS確認。アプリは 0.0.0.0 で待ち受ける設定に | 全環境 |
| マウントしたのにファイルが見えない | パス指定の誤り／PowerShellで $(pwd) を使った | PowerShellは "${PWD}:/app"。スペース入りパスは "" で囲む | PowerShell |
| 行末の \ が効かない | PowerShellでbash流の改行を使った | PowerShellは &#96; （バッククォート）に置き換える | PowerShell |
| 動作が極端に遅い（特にビルド/マウント） | /mnt/c 配下（Windows側）で作業している | プロジェクトを ~/ 配下（Linux側）へ移して作業 | Ubuntu/VSCode |
| exec: "bash": executable file not found | 軽量イメージ（alpine等）にbashが無い | docker exec -it 名前 sh を使う | 全環境 |
| no space left on device | イメージ・ボリュームで容量逼迫 | docker system df で確認 → docker system prune（内容を読んでから） | 全環境 |
| VSCodeでdockerコマンドが認識されない | 接続先がWSLでない／ターミナルの種類違い | 左下が「WSL: Ubuntu」か確認。違えば左下クリック→「WSLに接続」 | VSCode |

## 12. Ubuntu（bash）チートシート

WSLのUbuntu端末とVSCode（WSL接続）のターミナルで使います。

### 12-1. 移動・確認

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `pwd` | 今いる場所を表示 | `pwd` | print working directory |
| `cd` | フォルダ移動 | `cd ~/docker-lab` | cd .. で1つ上、cd ~ でホーム、cd - で直前の場所 |
| `ls` | 一覧表示 | `ls -la` | -l=詳細 / -a=隠しファイルも。権限や所有者も見える |
| `tree` | 階層表示 | `tree -L 2` | 無ければ sudo apt install tree |
| `ls -lh` | サイズを読みやすく表示 | `ls -lh` | K／M／G 単位 |
| `ls -lt` | 新しい順に表示 | `ls -lt \| head` | 更新日時順 |
| `pushd／popd` | 場所を積んで移動／戻る | `pushd /etc && popd` | cd - でも直前に戻れる |

### 12-2. ファイル操作

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `mkdir` | フォルダ作成 | `mkdir -p a/b/c` | -p で階層まとめて作成 |
| `touch` | 空ファイル作成 | `touch index.html` | 既存ファイルなら更新日時だけ変わる |
| `cp` | コピー | `cp a.txt b.txt ／ cp -r dir1 dir2` | フォルダは -r |
| `mv` | 移動・名前変更 | `mv a.txt b.txt` | 上書きに注意（-i で確認） |
| `rm` | 削除 | `rm a.txt ／ rm -r dir` | ⚠ゴミ箱に入らず戻せない。rm -rf は特に慎重に |
| `ln -s` | シンボリックリンク作成 | `ln -s ~/projects/app ~/app` | ショートカットのようなもの |
| `cp -a` | 属性ごとコピー | `cp -a src dst` | 権限・日時も保持 |
| `rm -i` | 確認しながら削除 | `rm -i a.txt` | 新人は -i を習慣に |
| `mkdir -p ... && cd` | 作ってすぐ移動 | `mkdir -p ~/lab && cd ~/lab` |  |
| `stat` | 詳細情報（権限・更新日時等） | `stat compose.yaml` |  |
| `realpath／basename／dirname` | パスの絶対化・名前部分・フォルダ部分 | `realpath .` | スクリプト向け |

### 12-3. 中身を見る

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `cat` | 中身を全部表示 | `cat compose.yaml` | 短いファイル向き |
| `less` | ページ送りで表示 | `less big.log` | スペースで次、q で終了、/語 で検索 |
| `head／tail` | 先頭／末尾を表示 | `tail -n 50 app.log ／ tail -f app.log` | tail -f は追跡表示（docker logs -f に近い） |
| `grep` | 文字列検索 | `grep -rn "error" .` | -r=フォルダ内 / -n=行番号 / -i=大小無視。docker logs web \| grep error |
| `echo` | 文字を表示・書き込み | `echo '<h1>Hi</h1>' > index.html` | > は上書き、>> は追記 |
| `wc -l` | 行数を数える | `wc -l app.log` | docker logs web \| wc -l |
| `sort／uniq` | 並べ替え／重複の集計 | `sort a.txt \| uniq -c` | ログの集計に |
| `cut` | 列を切り出す | `cut -d, -f1 a.csv` | -d=区切り -f=列 |
| `tee` | 画面に出しつつファイルにも保存 | `docker logs web \| tee web.log` | -a で追記 |
| `diff` | 2つのファイルの差分 | `diff a.yaml b.yaml` | 変更点の確認に。-u で見やすく |
| `file` | ファイル種別を調べる | `file a.txt` | 文字コード確認にも |

### 12-4. 編集

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `nano` | ターミナル内エディタ | `nano Dockerfile` | Ctrl+O 保存→Enter、Ctrl+X 終了 |
| `code .` | VSCodeで開く | `code .` | WSL上のフォルダをVSCode(WSL接続)で開く |
| `nano ~/.bashrc` | 起動時の設定ファイルを編集 | `nano ~/.bashrc` | alias を書く場所 |
| `source` | 設定ファイルを再読み込み | `source ~/.bashrc` | 編集後に実行 |
| `alias` | 短縮名を付ける | `alias d='docker'` | ~/.bashrc に書くと永続化 |

### 12-5. 調べる

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `which／type` | コマンドの場所 | `which docker` | docker が見つかるかの確認 |
| `man／--help` | 使い方 | `man ls ／ ls --help` | man は q で終了 |
| `history` | 入力履歴 | `history \| tail -20` | !番号 で再実行。Ctrl+R で履歴検索 |
| `echo $?` | 直前のコマンドの終了コード | `docker ps; echo $?` | 0=成功、それ以外=失敗（Dockerの Exited 番号と同じ考え方） |
| `cat /etc/os-release` | Ubuntuのバージョン詳細 | `cat /etc/os-release` |  |
| `env／printenv` | 環境変数の一覧 | `printenv \| sort` | docker run -e の確認にも |
| `echo $PATH` | コマンド検索パスを確認 | `echo $PATH \| tr ':' '\n'` | docker が見つからない時 |
| `type -a` | コマンドの種類・場所 | `type -a docker` | alias か実体かが分かる |
| `dpkg -l \| grep docker` | 導入済みパッケージ確認 | `dpkg -l \| grep docker` |  |
| `uptime／lsblk` | 稼働時間／ディスク構成 | `uptime` |  |
| `watch` | コマンドを一定間隔で再実行 | `watch -n 2 docker ps` | Ctrl+C で終了 |
| `time` | 実行時間を測る | `time docker compose up -d` |  |
| `sleep` | 待つ | `sleep 5` | スクリプト内で起動待ちに |

### 12-6. 権限

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `sudo` | 管理者権限で実行 | `sudo apt update` | パスワードは入力しても表示されない（正常） |
| `chmod` | 権限変更 | `chmod +x run.sh` | Exited (126) の対処にも |
| `chown` | 所有者変更 | `sudo chown -R $USER:$USER .` | Dockerが作ったファイルがroot所有になった時に |
| `id／groups` | 自分のユーザーとグループ | `groups` | docker が含まれていれば sudo 無しで docker が使える |
| `sudo usermod -aG docker $USER` | dockerグループに参加 | `sudo usermod -aG docker $USER` | 実行後は wsl --shutdown（PowerShell）→ 開き直し |
| `chmod 755／644／600` | 数字での権限指定 | `chmod 600 ~/.ssh/id_ed25519` | 755=実行可 644=通常 600=自分だけ |
| `su／sudo -i` | ユーザー／rootに切り替え | `sudo -i` | 作業後は exit で戻る |
| `whoami／id` | 今のユーザー確認 | `id` | dockerグループの有無 |

### 12-7. パッケージ（apt）

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `sudo apt update` | パッケージ一覧を更新 | `sudo apt update` | install の前にまず実行 |
| `sudo apt upgrade -y` | インストール済みを更新 | `sudo apt upgrade -y` | 最初に1回やっておくと安心 |
| `sudo apt install` | インストール | `sudo apt install -y curl tree` | -y で確認を省略 |
| `apt list --installed` | インストール済み一覧 | `apt list --installed \| grep docker` | docker関連の導入状況確認 |
| `apt search／apt show` | 検索／詳細表示 | `apt search docker` | 導入前に確認 |
| `sudo apt remove／purge` | 削除／設定ごと削除 | `sudo apt remove -y tree` |  |
| `sudo apt autoremove` | 不要な依存の削除 | `sudo apt autoremove -y` |  |
| `curl -fsSL https://get.docker.com \| sh` | Docker Engineの簡易インストール（Ubuntu内に直接導入） | `curl -fsSL https://get.docker.com -o get-docker.sh` | ⚠ スクリプトの中身を読んでから sudo sh get-docker.sh を実行。公式の手順も確認 |

### 12-8. プロセス・サービス

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `ps aux` | プロセス一覧 | `ps aux \| grep docker` | docker ps とは別物（OS全体のプロセス） |
| `top／htop` | リソース監視 | `top` | q で終了。重い時に |
| `sudo service docker status` | Dockerサービスの状態 | `sudo service docker status` | 直接導入（Docker Desktop無し）の場合 |
| `sudo service docker start` | Dockerサービスの起動 | `sudo service docker start` | WSLを開くたびに必要な場合あり |
| `kill` | プロセス終了 | `kill 1234` | 強制は kill -9 1234（最終手段） |
| `sudo systemctl enable --now docker` | 起動と自動起動を設定 | `sudo systemctl enable --now docker` | WSLでは systemd=true の設定後に使える（wsl.conf） |
| `systemctl status／restart` | サービスの状態／再起動 | `systemctl status docker` | systemd無効の場合は service コマンドを使う |
| `journalctl -u docker` | サービスのログ | `sudo journalctl -u docker -n 50` | Dockerデーモンのエラー確認 |
| `jobs／fg／bg` | バックグラウンドの処理管理 | `command &  → jobs → fg` | Ctrl+Z で一時停止 |
| `nohup` | ログアウト後も実行継続 | `nohup ./run.sh &` | 出力は nohup.out |

### 12-9. ネットワーク

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `curl` | HTTPアクセス | `curl -I http://localhost:8080` | Docker公開ポートの疎通確認。無ければ apt install curl |
| `ss -tulpn` | 待ち受けポート一覧 | `sudo ss -tulpn \| grep 8080` | port is already allocated の犯人探し |
| `ip a` | IPアドレス確認 | `ip a` | WSLのIPなど |
| `ip route／ping -c 3` | 経路／疎通確認 | `ping -c 3 8.8.8.8` | -c=回数（止まらないのを防ぐ） |
| `nslookup／dig` | 名前解決 | `nslookup example.com` | dig は dnsutils |
| `curl -v／-o /dev/null -w` | 詳細表示／ステータスだけ取得 | `curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080` | 200 なら到達OK |

### 12-10. システム

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `df -h` | ディスクの空き | `df -h` | -h=読みやすい単位。Dockerの容量不足時に |
| `du -sh` | フォルダの容量 | `du -sh *` | 大きいフォルダ探し |
| `free -h` | メモリ使用量 | `free -h` | Exited (137) の調査に |
| `uname -a／lsb_release -a` | OS情報 | `lsb_release -a` | Ubuntuのバージョン確認 |
| `date` | 日時表示 | `date` | ログの時刻とのずれ確認 |
| `exit` | ターミナル／コンテナから抜ける | `exit` | コンテナに入った時も同じ |
| `dmesg \| tail` | カーネルのメッセージ | `dmesg \| tail` | メモリ不足(OOM)の確認 |
| `ulimit -n` | 開けるファイル数の上限 | `ulimit -n` |  |

### 12-11. キー操作

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `Tab` | 補完 | `Tab` | コマンド・ファイル名・docker名も補完。2回押すと候補一覧 |
| `↑／↓` | 履歴の呼び出し | `↑` | 直前のコマンドを再利用 |
| `Ctrl+C` | 実行を中断 | `Ctrl+C` | logs -f／stats などを抜ける |
| `Ctrl+L` | 画面クリア | `Ctrl+L` | clear と同じ |
| `Ctrl+Shift+C／V` | コピー／貼り付け（Ubuntu端末） | `Ctrl+Shift+V` | Ubuntu端末では Ctrl+V が効かないことがある |
| `Ctrl+A／Ctrl+E` | 行頭／行末へ移動 | `Ctrl+A` | 長いコマンドの修正に便利 |
| `Ctrl+D` | 入力終了／ログアウト | `Ctrl+D` | exit と同じ効果 |
| `Ctrl+R` | 履歴の検索 | `Ctrl+R → docker と入力` | もう一度 Ctrl+R で次の候補 |
| `Ctrl+W／Ctrl+U／Ctrl+K` | 直前の単語／行頭まで／行末まで削除 | `Ctrl+U` |  |
| `Ctrl+Z` | 処理の一時停止 | `Ctrl+Z` | fg で再開 |
| `Alt+.` | 直前のコマンドの最後の引数を挿入 | `Alt+.` |  |

### 12-12. 検索・加工

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `find` | ファイル検索 | `find . -name "*.yaml"` | -type f／-type d、-mtime -1（1日以内） |
| `grep -r` | フォルダ内を再帰検索 | `grep -rn "8080" .` | 設定値の場所探しに |
| `grep -v／-c` | 除外／件数 | `docker ps -a \| grep -v Exited` | -v=含まない行 |
| `xargs` | 前の出力を引数にする | `find . -name "*.log" \| xargs rm` | ⚠ rm と組み合わせる時は先に件数・一覧を確認 |
| `awk` | 列を取り出す・加工 | `docker ps \| awk '{print $1}'` | $1=1列目 |
| `sed` | 置換 | `sed -i 's/8080/8081/' compose.yaml` | -i=ファイルを直接書き換え。先にコピーを取る |
| `jq` | JSONの整形・抽出 | `docker inspect web \| jq '.[0].State.Status'` | sudo apt install jq |
| `lsof -i` | ポートを使うプロセス | `sudo lsof -i :8080` | ss -tulpn の代わりにも |

### 12-13. 圧縮・ダウンロード

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `tar czf／tar xzf` | 圧縮／展開 | `tar czf a.tgz dir ／ tar xzf a.tgz` | c=作る x=展開 z=gzip f=ファイル |
| `zip／unzip` | zipの作成・展開 | `unzip a.zip` | sudo apt install zip unzip |
| `wget` | ファイルのダウンロード | `wget https://example.com/a.zip` |  |
| `curl -O／-L` | ダウンロード／リダイレクト追従 | `curl -L -O https://example.com/a.zip` | -o ファイル名 で保存名を指定 |
| `scp／rsync` | 他のマシンへコピー | `scp a.txt user@host:/tmp/` | rsync は差分だけ転送 |

### 12-14. シェルの便利技

| コマンド | 意味 | 使用例 | 補足 |
|---|---|---|---|
| `!!／sudo !!` | 直前のコマンドを再実行／sudo付きで再実行 | `sudo !!` | 権限不足で失敗した直後に |
| `リダイレクト` | 出力の保存・振り分け | `docker logs web > out.log 2>&1` | > 上書き / >> 追記 / 2>&1 エラーも一緒に |
| `/dev/null` | 出力を捨てる | `command > /dev/null 2>&1` |  |
| `&&／\|\|／;` | 連結 | `docker stop web && docker rm web` | &&=成功したら / \|\|=失敗したら / ;=無条件 |
| `$( )` | コマンドの結果を埋め込む | `docker rm $(docker ps -aq -f status=exited)` | 実行前に中身だけ echo で確認 |
| `for／while` | 繰り返し | `for c in web db; do docker restart $c; done` |  |
| `~ ／ $HOME` | ホームフォルダ | `cd ~` |  |

## 13. VSCode チートシート

### 13-1. ショートカット（Windows）

| 分類 | 操作 | ショートカット | 補足 |
|---|---|---|---|
| 基本 | コマンドパレットを開く | Ctrl+Shift+P | 何でも検索して実行。迷ったらこれ（F1でも可） |
| 基本 | ファイルを検索して開く | Ctrl+P | ファイル名の一部で検索 |
| 基本 | 保存 | Ctrl+S | 自動保存は 設定→Auto Save |
| 基本 | 設定を開く | Ctrl+, | 検索ボックスで設定を探せる |
| 基本 | 元に戻す／やり直し | Ctrl+Z／Ctrl+Y |  |
| 画面 | サイドバーの表示切替 | Ctrl+B | 画面を広く使いたい時 |
| 画面 | エクスプローラー | Ctrl+Shift+E | ファイル一覧 |
| 画面 | 拡張機能 | Ctrl+Shift+X | インストール・管理 |
| 画面 | エディターを左右に分割 | Ctrl+\ | 隣に別ファイルを並べる |
| 画面 | タブを閉じる | Ctrl+W |  |
| ターミナル | ターミナルの開閉 | Ctrl+@（または Ctrl+&#96;） | Ctrl+@ は日本語キーボードでの表記。効かない時は Ctrl+&#96; を試す |
| ターミナル | 新しいターミナル | Ctrl+Shift+&#96; | 複数同時に使える（logs用／操作用など） |
| ターミナル | ターミナルを分割 | Ctrl+Shift+5 | 横並びで logs -f を見ながら操作 |
| ターミナル | コピー／貼り付け | Ctrl+C（選択時）／Ctrl+V | 選択していない時の Ctrl+C は処理の中断 |
| ターミナル | クリア | clear（bash）／cls（PowerShell） | Ctrl+L の場合もある |
| 編集 | 行を移動 | Alt+↑／Alt+↓ | コピペせず並べ替え |
| 編集 | 行を複製 | Shift+Alt+↓ | compose.yaml の編集で便利 |
| 編集 | 行を削除 | Ctrl+Shift+K |  |
| 編集 | コメントON/OFF | Ctrl+/ | Dockerfile・YAMLは # コメント |
| 編集 | 同じ語を順に選択 | Ctrl+D | 複数カーソルで一括編集 |
| 編集 | インデント増減 | Tab／Shift+Tab | YAMLはスペース2つ（タブ文字は不可） |
| 編集 | 整形 | Shift+Alt+F | フォーマッタ導入時 |
| 編集 | 名前の一括変更 | F2 | 変数名・関数名など |
| 検索 | ファイル内の検索／置換 | Ctrl+F／Ctrl+H |  |
| 検索 | プロジェクト全体を検索 | Ctrl+Shift+F | 設定値の場所探しに |
| Markdown | プレビュー（別タブ） | Ctrl+Shift+V | このチートシートもプレビューで読める |
| Markdown | プレビュー（横並び） | Ctrl+K → V | 押してから離して V |

### 13-2. VSCode × WSL × Docker の操作

| やりたいこと | 操作方法 | 補足 |
|---|---|---|
| UbuntuのフォルダをVSCodeで開く | Ubuntuで cd ~/プロジェクト → code . | 左下が「WSL: Ubuntu」になっていることを確認 |
| WSLに接続する（VSCode側から） | Ctrl+Shift+P →「WSL: Connect to WSL」 | 接続先が違うと docker が見つからない原因に |
| 今の接続先を確認 | 画面左下の緑／青の表示 | 「WSL: Ubuntu」ならUbuntu側。表示なしはWindows側 |
| ターミナルの種類を切り替える | ターミナル右上の「＋の横の ∨」→ 種類を選択 | bash／PowerShell など。今どれか必ず確認 |
| 既定のターミナルを変える | Ctrl+Shift+P →「Terminal: Select Default Profile」 | 毎回PowerShellが開いて困る時に |
| Dockerの一覧を見る | 左のサイドバーのクジラアイコン | Containers／Images／Volumes／Networks をクリックで確認 |
| コンテナのログを見る | Containers で右クリック →「View Logs」 | docker logs と同じ |
| コンテナの中に入る | Containers で右クリック →「Attach Shell」 | docker exec -it の代わり |
| コンテナの起動・停止・削除 | Containers で右クリック → Start／Stop／Remove | コマンドを覚えるまでの補助に |
| イメージをビルド | Dockerfile を右クリック →「Build Image...」 | 名前:タグを入力 |
| Composeで起動 | compose.yaml を右クリック →「Compose Up」 | docker compose up と同じ |
| Dockerfile／Composeの入力補完 | Dockerfile や compose.yaml を編集 | Docker拡張機能があると命令やキーが補完される |
| Dev Container（開発環境を丸ごとコンテナ化） | Ctrl+Shift+P →「Dev Containers: Reopen in Container」 | 要 .devcontainer/devcontainer.json。チームの環境統一に有効 |
| Dev Containerから戻る | Ctrl+Shift+P →「Dev Containers: Reopen Folder Locally」（またはWSLで再度開く） | 左下の表示で現在の接続先を確認 |

### 13-3. 入れておきたい拡張機能

> 拡張機能は名前で検索し、**発行元が Microsoft 等の信頼できるもの** か確認してから入れましょう。

| 拡張機能 | 拡張機能ID | 用途 |
|---|---|---|
| WSL | `ms-vscode-remote.remote-wsl` | VSCodeからUbuntu（WSL）に接続して開発する |
| Docker | `ms-azuretools.vscode-docker` | Docker操作をGUIで行う。Dockerfile／compose の補完 |
| Dev Containers | `ms-vscode-remote.remote-containers` | コンテナの中でVSCodeを使う |
| YAML | `redhat.vscode-yaml` | compose.yaml の文法チェック・補完 |
| Markdown All in One | `yzhang.markdown-all-in-one` | Markdownの目次・整形・ショートカット |
| Japanese Language Pack | `MS-CEINTL.vscode-language-pack-ja` | VSCodeの表示を日本語に |

### 13-4. .devcontainer/devcontainer.json ひな形

```json
{
  "name": "my-dev",
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "customizations": {
    "vscode": {
      "extensions": ["ms-azuretools.vscode-docker"]
    }
  }
}
```

## 14. PowerShell チートシート

### 14-1. 基本コマンド（コマンドレット）

#### 14-1-1. 移動・確認

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Get-Location` | pwd | pwd | `pwd` | 今いるフォルダを表示 |
| `Set-Location` | cd | cd | `cd C:\Users` | フォルダ移動。cd .. で1つ上へ |
| `Get-ChildItem` | ls／dir | ls | `ls -Force` | 一覧。-Force で隠しファイルも |
| `Test-Path` | — | test -e | `Test-Path .\compose.yaml` | 存在チェック（True/False） |
| `Push-Location／Pop-Location` | pushd／popd | pushd／popd | `pushd C:\temp; popd` | 場所を積んで移動／戻る |
| `Get-ChildItem -Recurse -Filter` | ls -r | find | `ls -Recurse -Filter *.yaml` | サブフォルダも検索 |
| `Get-Item` | gi | stat | `Get-Item compose.yaml` | ファイルの詳細（LastWriteTimeなど） |
| `Resolve-Path` | rvpa | realpath | `Resolve-Path .` | 絶対パスにする |

#### 14-1-2. ファイル操作

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `New-Item -ItemType Directory` | mkdir | mkdir | `mkdir docker-lab` | フォルダ作成 |
| `New-Item` | ni | touch | `New-Item index.html` | 空ファイル作成 |
| `Copy-Item` | cp／copy | cp | `cp a.txt b.txt` | コピー。フォルダは -Recurse |
| `Move-Item` | mv／move | mv | `mv a.txt b.txt` | 移動・名前変更 |
| `Remove-Item` | rm／del | rm | `rm a.txt` | 削除。フォルダは -Recurse -Force（⚠戻せない） |
| `Rename-Item` | ren | mv | `ren a.txt b.txt` | 名前変更 |
| `New-Item -ItemType File -Force` | ni | touch -p | `ni a\b\c.txt -Force` | フォルダごと作成 |
| `Copy-Item -Recurse` | cp -r | cp -r | `cp src dst -Recurse` | フォルダごとコピー |
| `Add-Content` | ac | echo >> | `Add-Content a.txt "追記"` | 追記 |
| `Out-File -Append` | — | >> | `"追記" \| Out-File a.txt -Append` | 追記 |

#### 14-1-3. 中身を見る

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Get-Content` | cat／type | cat | `cat compose.yaml` | 中身表示。-Tail 20 で末尾20行、-Wait で追跡 |
| `Select-String` | sls | grep | `docker logs web \| sls error` | 文字列検索（grep相当） |
| `Set-Content／Out-File` | — | > ファイル | `'hello' \| Out-File a.txt` | 書き込み。文字化け時は -Encoding utf8 |
| `Get-Content -Tail -Wait` | gc | tail -f | `gc app.log -Tail 20 -Wait` | 末尾を追跡表示（Ctrl+Cで終了） |
| `Select-String -Path` | sls | grep -rn | `sls -Path .\*.yaml -Pattern 8080` | ファイル内検索（行番号つき） |
| `Compare-Object` | compare | diff | `Compare-Object (gc a.txt) (gc b.txt)` | 差分比較 |

#### 14-1-4. 調べる

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Get-Command` | gcm | which | `gcm docker` | コマンドの場所。docker が見つかるか確認 |
| `Get-Help` | help／man | man | `Get-Help ls` | ヘルプ。-Examples で例 |
| `Get-Process` | ps | ps | `Get-Process docker*` | プロセス一覧 |
| `Get-History` | h | history | `h` | 入力履歴。↑キーでも呼び出せる |
| `Test-NetConnection` | — | nc／telnet | `Test-NetConnection localhost -Port 8080` | ポート到達確認（Docker公開ポートの確認に便利） |

#### 14-1-5. パイプ・整形

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Where-Object` | where／? | grep的絞り込み | `ls \| ? Length -gt 1KB` | 条件で絞り込み |
| `Select-Object` | select | cut／head | `ls \| select -First 5` | 列・件数を絞る |
| `Sort-Object` | sort | sort | `ls \| sort Length` | 並べ替え |
| `Format-Table` | ft | column -t | `ls \| ft Name,Length` | 表で整形 |
| `Out-Host -Paging／more` | more | less | `cat big.log \| more` | 1画面ずつ表示（qで終了） |
| `Format-List` | fl | — | `docker ps \| fl` | 項目を縦に表示 |
| `Select-Object -ExpandProperty` | — | cut | `ls \| select -Expand Name` | 値だけ取り出す |
| `Sort-Object -Unique` | sort -u | sort -u | `gc a.txt \| sort -Unique` | 重複を除いて並べ替え |
| `Group-Object` | group | sort \| uniq -c | `gc a.txt \| group` | 件数の集計 |
| `Measure-Object -Sum` | measure | wc／awk | `ls \| measure Length -Sum` | 合計・平均 |
| `Out-GridView` | ogv | — | `ls \| ogv` | GUIの表で絞り込み（Windows PowerShell） |

#### 14-1-6. 変数・環境

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `$変数名` | — | 変数=値 | `$name = "web"` | 変数。docker rm $name のように使える |
| `$env:名前` | — | export | `$env:TZ = "Asia/Tokyo"` | 環境変数（そのウィンドウ内のみ有効） |
| `$PWD` | — | $(pwd) | `"${PWD}:/app"` | 現在のフォルダ。-v で使う |
| `$HOME／~` | — | ~ | `cd ~` | ホームフォルダ |
| `[Environment]::SetEnvironmentVariable` | — | setx／export | `[Environment]::SetEnvironmentVariable("NAME","web","User")` | 環境変数の永続設定（次回起動から有効） |
| `Get-ChildItem Env:` | gci env: | printenv | `gci env:` | 環境変数の一覧 |
| `$env:Path` | — | $PATH | `$env:Path -split ';'` | PATHを1行ずつ表示 |
| `Set-Alias／function` | sal | alias | `function d { docker @args }` | よく使う短縮を作る（$PROFILE に書く） |

#### 14-1-7. 実行・権限

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `管理者として実行` | — | sudo | `右クリック→管理者として実行` | wsl --install の時などに必要 |
| `Get-ExecutionPolicy` | — | — | `Get-ExecutionPolicy` | スクリプト(.ps1)の実行ルール確認 |
| `Set-ExecutionPolicy` | — | — | `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` | スクリプトが動かない時。意味を理解して実行 |

#### 14-1-8. 画面・キー

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Clear-Host` | cls／clear | clear | `cls` | 画面クリア |
| `Tab` | — | Tab | `Tab` | 補完。コマンド名・ファイル名・パラメータも補完される |
| `Ctrl+C` | — | Ctrl+C | `Ctrl+C` | 実行中の処理を中断（logs -f の終了など） |
| `F7` | — | Ctrl+R | `F7` | 履歴一覧から選択（Ctrl+Rで履歴検索もできる環境あり） |

#### 14-1-9. ネットワーク・ポート

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Test-Connection` | ping | ping | `Test-Connection localhost -Count 2` | 疎通確認（pingと同じ。ping も使える） |
| `Get-NetTCPConnection` | — | ss／netstat | `Get-NetTCPConnection -LocalPort 8080` | ポートを使っているプロセスのID(OwningProcess)が分かる。port is already allocated の原因調査 |
| `Get-Process -Id` | — | ps -p | `Get-Process -Id 1234` | プロセスIDから実体を調べる（上のコマンドと組み合わせる） |
| `Stop-Process` | kill | kill | `Stop-Process -Id 1234` | プロセス終了。強制は -Force（⚠重要なプロセスに注意） |
| `Invoke-WebRequest` | iwr／curl | curl | `iwr http://localhost:8080 -UseBasicParsing` | HTTPアクセス。StatusCode が 200 なら到達OK |
| `Invoke-RestMethod` | irm | curl（JSON） | `irm http://localhost:8080/api` | API結果をオブジェクトで取得 |
| `Get-NetIPAddress` | — | ip a | `Get-NetIPAddress -AddressFamily IPv4` | IPアドレス一覧（ipconfig も使える） |
| `Resolve-DnsName` | — | nslookup／dig | `Resolve-DnsName example.com` | 名前解決の確認 |

#### 14-1-10. システム・サービス

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `$PSVersionTable` | — | — | `$PSVersionTable.PSVersion` | PowerShellのバージョン確認（5.1 or 7）。&& が使えるか判断 |
| `Get-Service` | gsv | systemctl status | `Get-Service *docker*` | サービスの状態（Docker Desktop Service など） |
| `Start-Service／Stop-Service` | — | systemctl start／stop | `Start-Service com.docker.service` | 管理者のPowerShellが必要 |
| `Get-ComputerInfo` | — | uname -a | `Get-ComputerInfo \| select OsName,OsVersion` | OS情報（遅いことがある）。systeminfo でも可 |
| `Get-Date` | — | date | `Get-Date -Format "yyyy-MM-dd HH:mm"` | 日時の表示 |
| `whoami／hostname` | — | whoami／hostname | `whoami` | ユーザー名／PC名 |
| `Get-PSDrive` | — | df -h | `Get-PSDrive C` | ドライブの空き容量。Docker/WSLの容量不足時に |
| `Get-CimInstance Win32_OperatingSystem` | — | uname | `Get-CimInstance Win32_OperatingSystem` | OS情報。メモリ空きも見える |
| `Get-WindowsOptionalFeature` | — | — | `Get-WindowsOptionalFeature -Online \| ? State -eq Enabled` | 有効なWindowsの機能（管理者）。WSL／仮想化の確認に |
| `Enable-WindowsOptionalFeature` | — | — | `Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform` | WindowsのWSL関連機能を有効化（管理者・要再起動） |
| `Get-NetAdapter` | — | ip link | `Get-NetAdapter` | ネットワークアダプタ一覧 |

#### 14-1-11. パッケージ(winget)

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `winget search` | — | apt search | `winget search docker` | アプリを検索（Windows標準のパッケージ管理） |
| `winget install` | — | apt install | `winget install Docker.DockerDesktop` | インストール。IDは winget search で確認 |
| `winget list／upgrade` | — | apt list／upgrade | `winget upgrade --all` | 導入済み一覧／まとめて更新 |

#### 14-1-12. 便利・応用

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `Start-Process` | start／saps | xdg-open | `Start-Process http://localhost:8080` | ブラウザで開く／アプリ起動。管理者起動は -Verb RunAs |
| `Invoke-Item` | ii | explorer.exe | `ii .` | 現在のフォルダをエクスプローラーで開く |
| `Set-Clipboard／Get-Clipboard` | scb／gcb | xclip | `docker ps \| Set-Clipboard` | 出力をクリップボードへ（質問時に貼れる）。\| clip でも可 |
| `Measure-Object` | measure | wc | `cat a.log \| measure -Line` | 行数などを数える |
| `Tee-Object` | tee | tee | `docker logs web \| tee log.txt` | 画面表示しながらファイルにも保存 |
| `Compress-Archive／Expand-Archive` | — | zip／unzip | `Compress-Archive src out.zip` | zipの作成・展開 |
| `Get-FileHash` | — | sha256sum | `Get-FileHash a.zip` | ファイルのハッシュ値確認 |
| `Join-Path／Split-Path` | — | — | `Join-Path $HOME docker-lab` | パスの結合・分解（\の書き間違い防止） |
| `Get-Alias／Set-Alias` | gal／sal | alias | `Get-Alias ls` | 別名の確認・登録 |
| `Get-Member` | gm | — | `ls \| gm` | オブジェクトの持つ項目を調べる（PowerShellの重要な考え方） |
| `$PROFILE` | — | ~/.bashrc | `notepad $PROFILE` | 起動時に読み込まれる設定ファイル。よく使う別名の登録に |
| `Get-Command -Name docker*` | gcm | compgen -c | `gcm docker*` | docker で始まるコマンドの一覧 |
| `Update-Help` | — | — | `Update-Help` | ヘルプの更新（管理者） |
| `Get-Help -Online` | — | man | `Get-Help ls -Online` | ブラウザで公式ヘルプを開く |
| `$PROFILE の作成` | — | ~/.bashrc | `if (!(Test-Path $PROFILE)) { ni $PROFILE -Force }; notepad $PROFILE` | 設定ファイルが無い時の作り方 |

#### 14-1-13. 制御・演算子

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `$LASTEXITCODE` | — | $? | `docker ps; $LASTEXITCODE` | 直前の外部コマンド（docker等）の終了コード。0=成功 |
| `比較演算子` | — | [ ] ／ -eq | `$a -eq 1` | -eq（等しい）-ne（等しくない）-gt（より大きい）-lt（より小さい）-ge／-le -like（ワイルドカード）-match（正規表現）。== ではない点に注意 |
| `論理演算子` | — | && ／ \|\| | `($a -gt 1) -and ($b -lt 5)` | -and／-or／-not。（&&／\|\| は PowerShell 7 でコマンド連結用） |
| `if` | — | if | `if (Test-Path a.txt) { "ある" } else { "ない" }` | 条件分岐。条件は ( )、処理は { } |
| `foreach／ForEach-Object` | % | for | `1..3 \| % { "No.$_" }` | 繰り返し。$_ が現在の要素 |
| `try／catch` | — | — | `try { docker ps } catch { "失敗" }` | エラー処理 |
| `Write-Host／Read-Host` | echo | echo／read | `Read-Host "名前は？"` | 表示／キーボード入力の受け取り |

#### 14-1-14. JSON・データ加工

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `ConvertFrom-Json` | — | jq | `docker inspect web \| ConvertFrom-Json` | JSONをオブジェクトにする（Docker出力の解析に便利） |
| `ConvertTo-Json` | — | jq | `@{a=1} \| ConvertTo-Json` | オブジェクトをJSONにする |
| `(docker inspect web \| ConvertFrom-Json).State` | — | jq '.[0].State' | `(docker inspect web \| ConvertFrom-Json).State.Status` | 項目を直接取り出す |
| `Export-Csv／Import-Csv` | — | — | `Import-Csv skills_ledger.csv` | CSVの入出力（台帳の読み込み等） |
| `-Encoding UTF8` | — | — | `Import-Csv a.csv -Encoding UTF8` | 文字化けする時に指定 |

#### 14-1-15. 安全・記録

| コマンドレット | 別名 | bash相当 | 使用例 | 説明 |
|---|---|---|---|---|
| `-WhatIf` | — | --dry-run | `Remove-Item * -Recurse -WhatIf` | ⭐実行せず「何が起きるか」だけ表示。削除前の確認に |
| `-Confirm` | — | -i | `Remove-Item a.txt -Confirm` | 実行前に確認を出す |
| `Start-Transcript／Stop-Transcript` | — | script | `Start-Transcript -Path log.txt` | 操作の記録を保存（研修の復習・報告に） |
| `Start-Sleep` | sleep | sleep | `Start-Sleep -Seconds 5` | 待つ |
| `$?` | — | $? | `docker ps; $?` | 直前のコマンドの成否（True/False）。外部コマンドの終了コードは $LASTEXITCODE |

### 14-2. bash と PowerShell の書き方の違い（Docker操作編）

| やりたいこと | bash（Ubuntu/VSCode） | PowerShell | 注意 |
|---|---|---|---|
| コマンドの改行 | `行末に \` | 行末に &#96;（バッククォート） | バッククォートの後ろにスペースを入れない |
| 現在フォルダをマウント | `-v "$(pwd)":/app` | `-v "${PWD}:/app"` | スペース入りのパスは "" で囲む |
| コマンドを続けて実行（成功時のみ） | `cmd1 && cmd2` | `cmd1; cmd2（PowerShell 7 では && も可）` | Windows PowerShell 5.1 は && 不可 |
| 環境変数を設定 | `export KEY=value` | `$env:KEY="value"` | docker run -e KEY=value とは別物 |
| ログを検索 | `docker logs web \| grep error` | `docker logs web \| Select-String error` | 大文字小文字は区別されない |
| ログの末尾を追う | `docker logs -f web` | `docker logs -f web` | 同じ。Ctrl+Cで抜ける |
| コンテナを全部消す（⚠） | `docker rm -f $(docker ps -aq)` | `docker ps -aq \| ForEach-Object { docker rm -f $_ }` | ⚠削除前に docker ps -a で必ず確認 |
| 止まったコンテナのIDだけ取る | `docker ps -aq --filter "status=exited"` | `docker ps -aq --filter "status=exited"` | 同じ書き方で動く |
| HTTPで疎通確認 | `curl -I http://localhost:8080` | `curl.exe -I http://localhost:8080` | PowerShellの curl は Invoke-WebRequest の別名なので curl.exe |
| ポート到達確認 | `nc -zv localhost 8080` | `Test-NetConnection localhost -Port 8080` | ncが無い環境ではこの違いに注意 |
| ファイルに書き込む | `echo '<h1>Hi</h1>' > index.html` | `'<h1>Hi</h1>' \| Out-File index.html -Encoding utf8` | 5.1ではBOM付きになる場合あり |
| ファイル作成 | `touch a.txt` | `New-Item a.txt` | — |
| フォルダ作成（階層） | `mkdir -p a/b` | `mkdir a\b` | PowerShellの mkdir は階層も作れる |
| ファイル削除（フォルダごと） | `rm -rf dir` | `Remove-Item dir -Recurse -Force` | ⚠戻せない。パスを確認 |
| 中身を見る | `cat compose.yaml` | `cat compose.yaml（Get-Contentの別名）` | 同じ見た目で使える |
| コマンドの場所 | `which docker` | `Get-Command docker` | docker が見つからない時の確認 |
| パスの区切り | `/home/user/app` | `C:\Users\user\app` | docker -v のWindowsパスは環境により書き方が変わるので ${PWD} が安全 |
| Ubuntuに入る／出る | `（WSL内）exit で出る` | `wsl -d Ubuntu で入る／exit で戻る` | プロンプトが変わったら今どこか確認 |

> 💡 **PowerShellの基本ルール**：コマンドは「動詞-名詞」（Get-ChildItem など）。別名（ls、cat、cd）も使えるので、bash経験者はそのまま打てる場面が多いです。

## 15. CMD（コマンドプロンプト）チートシート

Windowsに昔からある黒い画面です。Docker Desktopでは PowerShell が主流ですが、既存の手順書・バッチファイル（.bat）・古いPCでは CMD が使われます。

### 15-1. 移動・確認

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `cd` | フォルダ移動／今の場所を表示 | `cd C:\Users` | 引数なしで現在地表示。別ドライブへは cd /d D:\（または D: と入力） |
| `dir` | 一覧表示 | `dir /a` | /a=隠しファイルも / /s=サブフォルダも / /b=名前だけ。bash:ls／PowerShell:ls |
| `tree` | 階層表示 | `tree /f` | /f=ファイル名も表示 |
| `cls` | 画面クリア | `cls` | bash:clear |
| `ver` | Windowsのバージョン | `ver` |  |
| `whoami／hostname` | ユーザー名／PC名 | `whoami` |  |
| `where` | コマンドの場所 | `where docker` | bash:which／PowerShell:Get-Command |
| `dir /o:d` | 更新日時順に表示 | `dir /o:d` | /o:n=名前順 /o:s=サイズ順 |
| `dir /s /b *.yaml` | サブフォルダも含めて検索 | `dir /s /b *.yaml` | find 相当 |
| `where /r` | フォルダを指定してコマンド検索 | `where /r C:\ docker.exe` | 時間がかかる |
| `pushd／popd` | 場所を積んで移動／戻る | `pushd C:\temp & popd` | ネットワークドライブにも使える |
| `tree /a` | ASCII文字で階層表示 | `tree /a` | 文字化けする時に |

### 15-2. ファイル操作

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `mkdir／md` | フォルダ作成 | `mkdir docker-lab` | 階層もまとめて作成できる |
| `rmdir／rd` | フォルダ削除 | `rmdir /s /q old` | /s=中身ごと /q=確認なし（⚠戻せない） |
| `copy` | ファイルコピー | `copy a.txt b.txt` | bash:cp |
| `xcopy／robocopy` | フォルダごとコピー | `xcopy src dst /E /I` | robocopy は高機能。/MIR は削除も同期するので⚠ |
| `move` | 移動 | `move a.txt D:\backup\` | bash:mv |
| `ren` | 名前変更 | `ren a.txt b.txt` |  |
| `del` | ファイル削除 | `del /q a.txt` | フォルダは rmdir。/q=確認なし |
| `type nul >` | 空ファイル作成 | `type nul > a.txt` | bash:touch／PowerShell:New-Item |
| `attrib` | 属性の表示・変更 | `attrib +h a.txt` | +h=隠す／-h=解除 |
| `mklink` | リンク作成 | `mklink /D link target` | 管理者が必要な場合あり |
| `fc` | ファイルの差分比較 | `fc a.txt b.txt` | diff 相当 |
| `clip` | 出力をクリップボードへ | `docker ps \| clip` | 質問する時に貼れる |
| `attrib -r` | 読み取り専用の解除 | `attrib -r a.txt` |  |

### 15-3. 中身を見る

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `type` | 中身を表示 | `type compose.yaml` | bash:cat |
| `more` | ページ送りで表示 | `type big.log \| more` | スペースで次、q で終了 |
| `find` | 文字列検索（簡易） | `docker logs web \| find "error"` | 大文字小文字を区別。/i で区別なし |
| `findstr` | 文字列検索（高機能） | `docker logs web \| findstr /i error` | /i=大小無視 /n=行番号 /s=サブフォルダ。bash:grep |
| `echo` | 表示・書き込み | `echo Hello > a.txt` | > は上書き、>> は追記 |
| `sort` | 並べ替え | `type a.txt \| sort` |  |
| `more +N` | N行目から表示 | `more +10 big.log` |  |

### 15-4. 変数・環境

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `set` | 環境変数の設定／一覧 | `set NAME=web` | このウィンドウ内のみ有効。= の前後にスペースを入れない |
| `%変数名%` | 変数の参照 | `echo %NAME%` | bash:$NAME／PowerShell:$env:NAME |
| `setx` | 環境変数の永続設定 | `setx NAME web` | 次に開くウィンドウから有効（今のウィンドウは変わらない） |
| `%cd%` | 現在のフォルダ | `docker run -v "%cd%:/app" ...` | bash:$(pwd)／PowerShell:${PWD} |
| `%USERPROFILE%` | ホームフォルダ | `cd %USERPROFILE%` | C:\Users\ユーザー名 |
| `path` | PATH確認 | `path` | docker が見つからない時の確認 |
| `set /p` | 入力を受け取る | `set /p NAME=名前は？` | バッチ向け |
| `set /a` | 計算 | `set /a N=1+2` |  |
| `set 文字` | 先頭が一致する変数を表示 | `set P` | PATH、PROMPTなど |

### 15-5. ネットワーク

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `ipconfig` | IPアドレス確認 | `ipconfig /all` |  |
| `ping` | 疎通確認 | `ping localhost` | Ctrl+C で止める（-t は連続） |
| `tracert` | 経路確認 | `tracert example.com` |  |
| `nslookup` | 名前解決の確認 | `nslookup example.com` |  |
| `netstat -ano` | ポート使用状況とPID | `netstat -ano \| findstr :8080` | port is already allocated の原因探し。最後の数字がPID |
| `curl` | HTTPアクセス | `curl -I http://localhost:8080` | Windows 10以降は標準。CMDでは curl と書いてOK |
| `ipconfig /flushdns` | DNSキャッシュを消す | `ipconfig /flushdns` | 名前解決がおかしい時 |
| `netstat -an` | ポート状況（名前解決なし） | `netstat -an \| findstr LISTENING` | -o を付けるとPIDも出る |
| `route print` | ルーティングテーブル | `route print` |  |
| `curl -s -o NUL -w` | HTTPステータスだけ取得 | `curl -s -o NUL -w "%{http_code}" http://localhost:8080` | NUL は /dev/null 相当 |

### 15-6. プロセス・システム

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `tasklist` | プロセス一覧 | `tasklist \| findstr 1234` | PIDから実体を調べる |
| `taskkill` | プロセス終了 | `taskkill /PID 1234 /F` | /F=強制（⚠） |
| `systeminfo` | システム情報 | `systeminfo` | 「ハイパーバイザーが検出されました」等で仮想化の状態も分かる |
| `shutdown` | シャットダウン／再起動 | `shutdown /r /t 0` | /r=再起動 /s=終了 /t=待ち秒数。wsl --install 後の再起動に |
| `sfc /scannow` | システムファイルの修復 | `sfc /scannow` | 管理者のCMDが必要 |
| `timeout` | 待つ | `timeout /t 5` | バッチ内の待ち時間に |
| `start /b` | 別窓を開かずに実行 | `start /b docker compose up` | バックグラウンド実行 |
| `cmd /c／cmd /k` | コマンドを実行して終了／窓を残す | `cmd /k docker ps` |  |
| `choice` | 選択肢を出す | `choice /c YN /m "実行しますか"` | バッチ内の確認に。結果は %ERRORLEVEL% |

### 15-7. 起動・連携

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `start` | アプリ／URLを開く | `start http://localhost:8080` | ブラウザで開く。start . でエクスプローラー |
| `code .` | VSCodeで開く | `code .` | VSCodeがPATHに入っていれば使える |
| `wsl` | UbuntuのWSLに入る／WSL管理 | `wsl -d Ubuntu` | CMDからも wsl -l -v などが使える。exit で戻る |
| `powershell -Command` | CMDからPowerShellを実行 | `powershell -Command "Get-Date"` | PowerShellの機能を1行だけ使いたい時 |
| `start "" "パス"` | スペース入りパスのアプリ起動 | `start "" "C:\Program Files\...\app.exe"` | 最初の "" はウィンドウ名（必須） |
| `explorer .` | 現在のフォルダをエクスプローラーで開く | `explorer .` |  |
| `docker（CMDから）` | CMDからも docker が使える | `docker ps` | Docker Desktopが起動していることが前提 |

### 15-8. 連結・記号

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `&&／&／\|\|` | コマンドの連結 | `docker stop web && docker rm web` | &&=成功したら次 / &=常に次 / \|\|=失敗したら次 |
| `^` | 行継続 | `docker run -d ^` | 行末に付けて次の行へ続ける。^ の後ろにスペースを入れない（4-1章の例を参照） |
| `\|` | パイプ | `docker ps \| findstr web` | 前の出力を次の入力へ |
| `rem／::` | コメント | `rem これはコメント` | バッチファイル用 |
| `%ERRORLEVEL%` | 直前のコマンドの終了コード | `docker ps & echo %ERRORLEVEL%` | 0=成功。bash:$?／PowerShell:$LASTEXITCODE |

### 15-9. バッチファイル(.bat)

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `@echo off` | コマンドの表示を消す | `@echo off` | バッチの1行目に書く定番 |
| `pause` | キー入力待ち | `pause` | 実行結果を確認してから閉じる |
| `if exist` | ファイルの有無で分岐 | `if exist compose.yaml echo あり` |  |
| `for` | 繰り返し | `for %i in (a b c) do echo %i` | バッチファイル内では %%i と2つ書く |
| `call／goto／exit /b` | 他のバッチ呼び出し／ジャンプ／終了 | `call other.bat` |  |

### 15-10. キー操作

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `F7` | 入力履歴の一覧 | `F7` | ↑↓で選んで Enter |
| `F3／↑` | 直前のコマンドを呼び出す | `F3` |  |
| `Tab` | 補完 | `Tab` | ファイル名・フォルダ名を補完 |
| `Esc` | 入力中の行をクリア | `Esc` |  |
| `Ctrl+C` | 実行中の処理を中断 | `Ctrl+C` | logs -f などを止める |

### 15-11. 表示・文字コード

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `chcp 65001` | 文字コードをUTF-8に | `chcp 65001` | 日本語が文字化けする時の対処。標準は 932（Shift_JIS） |
| `title／color` | ウィンドウ名／色 | `title Docker作業` | 色は color 0A 等 |
| `prompt $P$G` | プロンプト表示の変更 | `prompt $P$G` |  |

### 15-12. Windowsの機能（WSL）

| コマンド | 意味 | 使用例 | 補足・bash／PowerShellでは |
|---|---|---|---|
| `dism（WSL機能の有効化）` | WSLの機能を有効化 | `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart` | 管理者のCMDで実行。実行後は再起動 |
| `dism（仮想マシンプラットフォーム）` | WSL2に必要な機能を有効化 | `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` | 管理者のCMDで実行。実行後は再起動 |
| `dism（機能の状態確認）` | 機能の状態を表示 | `dism.exe /online /get-featureinfo /featurename:VirtualMachinePlatform` | State : Enabled なら有効 |

### 15-13. bash ／ PowerShell ／ CMD 対比表（Docker操作編）

> ⚠ CMDでは **シングルクォート ' が引用符として働きません**。`--format` などは "" で囲みましょう。

| やりたいこと | bash（Ubuntu/VSCode） | PowerShell | CMD | 注意 |
|---|---|---|---|---|
| 起動のしかた | `Ubuntuを開く` | `Win+R → powershell（または wt）` | `Win+R → cmd` | 3つとも Windows Terminal のタブで使える |
| 行の途中改行 | `行末に \` | 行末に &#96; | `行末に ^` | 後ろにスペースを入れると失敗する |
| 現在のフォルダ | `$(pwd)` | `${PWD}` | `%cd%` | docker -v で使う。"" で囲む |
| -v の書き方 | `-v "$(pwd)":/app` | `-v "${PWD}:/app"` | `-v "%cd%:/app"` | スペース入りパスは必ず "" で囲む |
| 変数を設定 | `export A=1` | `$env:A="1"` | `set A=1` | CMDは = の前後にスペース禁止 |
| 変数を参照 | `$A` | `$env:A` | `%A%` |  |
| コマンドを連続実行 | `cmd1 && cmd2` | `cmd1; cmd2（PowerShell 7 は && も可）` | `cmd1 && cmd2` | PowerShell 5.1 は && 不可 |
| コメント | `# コメント` | `# コメント` | `rem コメント（または ::）` |  |
| 直前の終了コード | `$?／echo $?` | `$LASTEXITCODE` | `%ERRORLEVEL%` | 0=成功 |
| 画面クリア | `clear` | `cls` | `cls` |  |
| 一覧 | `ls` | `ls（Get-ChildItem）` | `dir` |  |
| フォルダ作成 | `mkdir -p a/b` | `mkdir a\b` | `mkdir a\b` | どれも階層をまとめて作れる |
| 空ファイル作成 | `touch a.txt` | `New-Item a.txt` | `type nul > a.txt` |  |
| 中身を表示 | `cat a.txt` | `cat a.txt（Get-Content）` | `type a.txt` |  |
| 文字列検索 | `grep error` | `Select-String error` | `findstr /i error` | docker logs web \| 上のどれか |
| コマンドの場所 | `which docker` | `Get-Command docker` | `where docker` | dockerが見つからない時 |
| ポート使用確認 | `ss -tulpn \| grep 8080` | `Get-NetTCPConnection -LocalPort 8080` | `netstat -ano \| findstr :8080` | port is already allocated の調査 |
| プロセス終了 | `kill 1234` | `Stop-Process -Id 1234` | `taskkill /PID 1234 /F` | ⚠ 重要なプロセスに注意 |
| ブラウザで開く | `explorer.exe http://localhost:8080` | `Start-Process http://localhost:8080` | `start http://localhost:8080` | WSLからは explorer.exe が使える |
| HTTP疎通確認 | `curl -I http://localhost:8080` | `curl.exe -I http://localhost:8080` | `curl -I http://localhost:8080` | PowerShellの curl は別物なので curl.exe |
| エクスプローラーで開く | `explorer.exe .` | `ii . （または explorer .）` | `start . （または explorer .）` |  |
| 全コンテナID取得→削除（⚠） | `docker rm -f $(docker ps -aq)` | `docker ps -aq \| % { docker rm -f $_ }` | `for /f %i in ('docker ps -aq') do docker rm -f %i` | ⚠削除前に docker ps -a で必ず確認。.bat内では %%i |
| --format の引用符 | `'{{.Names}}' も "{{.Names}}" も可` | `'{{.Names}}' も "{{.Names}}" も可` | `"{{.Names}}" のみ（' は使えない）` | CMDで ' を使うと動かない原因になる |
| パスの区切り | `/` | `\` | `\` | docker の -v はコンテナ側は常に / |
| Dockerの起動確認 | `docker version` | `docker version` | `docker version` | 3つとも同じ。Server欄が出ればOK |

## 16. Windows ショートカットチートシート

Docker作業（Ubuntu・VSCode・PowerShell・ブラウザを行き来する）で役立つショートカットを、場面別にまとめました。

### 16-1. よく使う基本

| 操作 | ショートカット | 補足 |
|---|---|---|
| コピー／切り取り／貼り付け | Ctrl+C／Ctrl+X／Ctrl+V | Ubuntu端末は Ctrl+Shift+C／V のことが多い |
| 元に戻す／やり直す | Ctrl+Z／Ctrl+Y |  |
| すべて選択 | Ctrl+A |  |
| 検索（そのアプリ内） | Ctrl+F | ブラウザ・VSCode・エクスプローラーなど共通 |
| 保存／印刷／新規 | Ctrl+S／Ctrl+P／Ctrl+N |  |
| クリップボード履歴 | Win+V | 過去にコピーした複数の内容から選んで貼れる。コマンドの使い回しに便利 |
| 絵文字・記号パネル | Win+.（ピリオド） | Win+; でも可 |
| 処理の中断（ターミナル） | Ctrl+C | logs -f／stats などを抜ける。コピーとは別の意味になる点に注意 |

### 16-2. アプリ・ウィンドウ切替

| 操作 | ショートカット | 補足 |
|---|---|---|
| アプリを切り替える | Alt+Tab | Shiftを足すと逆順 |
| タスクビュー（全ウィンドウ一覧） | Win+Tab |  |
| アプリを閉じる | Alt+F4 | デスクトップ上で押すとシャットダウン画面が出る |
| タスクバーのN番目のアプリ起動／切替 | Win+1〜9 | ターミナル・VSCode・ブラウザを左から固定しておくと速い |
| デスクトップを表示 | Win+D | もう一度で元に戻る |
| 全ウィンドウを最小化／元に戻す | Win+M／Win+Shift+M |  |
| アクティブ以外を最小化 | Win+Home | もう一度で元に戻る |

### 16-3. ウィンドウ配置

| 操作 | ショートカット | 補足 |
|---|---|---|
| 最大化／元のサイズ・最小化 | Win+↑／Win+↓ | 画面分割で見比べる時の基本 |
| 左半分／右半分に配置 | Win+←／Win+→ | 左=VSCode・右=ブラウザ のように並べる（localhost確認に便利） |
| スナップレイアウトを開く | Win+Z | 3分割などのレイアウトを選べる（Windows 11） |
| 別のモニターへ移動 | Win+Shift+←／→ |  |

### 16-4. 仮想デスクトップ

| 操作 | ショートカット | 補足 |
|---|---|---|
| 新しいデスクトップを作成 | Win+Ctrl+D | 作業用／資料用で分けられる |
| デスクトップを切り替える | Win+Ctrl+←／→ |  |
| 今のデスクトップを閉じる | Win+Ctrl+F4 |  |

### 16-5. 起動・システム

| 操作 | ショートカット | 補足 |
|---|---|---|
| スタートメニュー／検索 | Win／Win+S | アプリ名を打って Enter で起動 |
| エクスプローラーを開く | Win+E |  |
| ファイル名を指定して実行 | Win+R | 下の「Win+R で起動」の表を参照 |
| 設定を開く | Win+I |  |
| クイックリンクメニュー | Win+X | 続けて A で ターミナル（管理者）。wsl --install の時に使う |
| タスクマネージャー | Ctrl+Shift+Esc | Docker Desktopのメモリ使用量確認にも |
| 画面ロック | Win+L | 離席時に必須 |
| クイック設定／通知センター | Win+A／Win+N | 音量・Wi-Fi・通知 |
| 表示切替（プロジェクター等） | Win+P | 複製／拡張／外部のみ |
| 画面が乱れた時の復旧 | Win+Ctrl+Shift+B | グラフィックドライバーを再起動 |

### 16-6. スクリーンショット

| 操作 | ショートカット | 補足 |
|---|---|---|
| 範囲を選んでキャプチャ | Win+Shift+S | エラー画面を切り取って質問する時の定番。クリップボードに入る |
| 全画面をクリップボードへ | PrtScn |  |
| 全画面をファイル保存 | Win+PrtScn | ピクチャ＞スクリーンショット フォルダに保存 |
| アクティブウィンドウのみコピー | Alt+PrtScn |  |
| 録画（Xbox Game Bar） | Win+Alt+R／Win+G | 手順を動画で残したい時 |

### 16-7. テキスト編集

| 操作 | ショートカット | 補足 |
|---|---|---|
| 単語単位でカーソル移動 | Ctrl+←／Ctrl+→ | 長いコマンドの修正に |
| 単語単位で削除 | Ctrl+Backspace | 行頭／行末へは Home／End |
| 範囲選択 | Shift+矢印／Ctrl+Shift+←→ | 単語単位で選択 |
| 先頭／末尾へ | Ctrl+Home／Ctrl+End |  |

### 16-8. 日本語入力（IME）

| 操作 | ショートカット | 補足 |
|---|---|---|
| 日本語／英数の切替 | 半角/全角 | ターミナル入力前は英数に切り替える（全角スペース・全角記号の混入に注意） |
| ひらがなに変換 | F6 |  |
| 全角カタカナ／半角カタカナ | F7／F8 |  |
| 全角英数／半角英数に変換 | F9／F10 | コマンド名の打ち間違いを直す時に便利 |
| 入力言語の切替 | Win+Space／Alt+Shift |  |

### 16-9. エクスプローラー

| 操作 | ショートカット | 補足 |
|---|---|---|
| 新しいフォルダー | Ctrl+Shift+N |  |
| 名前の変更 | F2 |  |
| アドレスバーへ移動 | Ctrl+L／Alt+D | \\wsl.localhost\Ubuntu と入力すると Ubuntu のファイルが見られる |
| 戻る／進む／1つ上へ | Alt+←／Alt+→／Alt+↑ |  |
| パスとしてコピー | Ctrl+Shift+C（Windows 11） | 選択したファイルのパスをコピー。docker -v の確認に |
| プロパティ | Alt+Enter |  |
| ごみ箱へ／完全に削除 | Delete／Shift+Delete | ⚠ Shift+Delete は元に戻せない |
| 検索 | Ctrl+E／F3 |  |

### 16-10. ブラウザ（Chrome／Edge）

| 操作 | ショートカット | 補足 |
|---|---|---|
| 新しいタブ／閉じる | Ctrl+T／Ctrl+W |  |
| 閉じたタブを復活 | Ctrl+Shift+T |  |
| タブ切替 | Ctrl+Tab／Ctrl+Shift+Tab |  |
| アドレスバーへ | Ctrl+L／F6 | http://localhost:8080 を開く時 |
| 再読み込み（キャッシュ無視） | F5／Ctrl+F5 | Dockerで更新が反映されない時は Ctrl+F5 |
| 開発者ツール | F12／Ctrl+Shift+I | ネットワークタブで通信確認 |
| シークレット／InPrivateウィンドウ | Ctrl+Shift+N／Ctrl+Shift+P | キャッシュの影響を避けて確認できる |

### 16-11. Windows Terminal

| 操作 | ショートカット | 補足 |
|---|---|---|
| 新しいタブ | Ctrl+Shift+T | 既定のプロファイル（Ubuntu／PowerShell）で開く |
| プロファイルを指定して新しいタブ | Ctrl+Shift+1〜9 | 設定の並び順の番号。Ubuntu・PowerShellを一発で開く |
| タブを閉じる／タブ切替 | Ctrl+Shift+W／Ctrl+Tab |  |
| ペインを分割 | Alt+Shift++（縦に分割）／Alt+Shift+-（横に分割） | 片方で docker logs -f、片方で操作 |
| ペイン間の移動 | Alt+矢印キー |  |
| コピー／貼り付け | Ctrl+Shift+C／Ctrl+Shift+V | Ctrl+V でも貼り付け可能な設定が多い |
| 画面内の検索 | Ctrl+Shift+F | ログの中の error を探す時に |
| 文字サイズ変更 | Ctrl+（+）／Ctrl+（-） | 研修の投影時に拡大 |
| コマンドパレット | Ctrl+Shift+P |  |
| 全画面表示 | F11／Alt+Enter |  |

### 16-12. Win+R で起動

| 操作 | 入力する名前 | 補足 |
|---|---|---|
| コマンドプロンプト | cmd |  |
| PowerShell | powershell | 管理者で開く場合は Win+X → A |
| Windows Terminal | wt | Ubuntu・PowerShell をタブで使える |
| WSL（既定のUbuntu） | wsl | Ubuntuのbashが開く |
| UbuntuのファイルをExplorerで開く | \\wsl.localhost\Ubuntu | Ubuntuのホームは home\ユーザー名 |
| Windowsの機能の有効化／無効化 | optionalfeatures | WSL・仮想マシンプラットフォームの有効化はここ（要再起動） |
| アプリと機能（アンインストール） | appwiz.cpl | Docker Desktopのアンインストール等 |
| サービス | services.msc | Docker Desktop Service の状態確認 |
| システム情報 | msinfo32 | 仮想化が有効かの確認など |
| 環境変数の編集 | sysdm.cpl | 詳細設定 → 環境変数 |
| タスクマネージャー | taskmgr | パフォーマンス → 仮想化 が「有効」か確認（Docker/WSL2に必須） |

## 17. Claude Code コマンドチートシート

> ⚠ Claude Code は更新が早く、コマンドの有無やオプションはバージョン・プランで変わります。最終確認は、Claude Code内で `/help`、および公式ドキュメント（https://docs.claude.com/en/docs/claude-code/overview）で行ってください。

### 17-1. WSL（Ubuntu）で使う時のポイント

- **Ubuntu（WSL）の中で** `claude` を起動し、プロジェクトは `~/` 配下に置くと速く安定します（`/mnt/c` 配下は遅くなりがち）。
- WSLの `~/.claude/` と、Windows側の `C:\Users\ユーザー名\.claude\` は **別のフォルダ** です。スキルは、使う側のフォルダに置きます。
- VSCodeの統合ターミナル（WSL接続）で `claude` を起動すれば、エディタと並べて使えます。

### 17-2. スラッシュコマンド（セッション内で `/` から始める）

#### 17-2-1. 基本・セッション

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/help` | 使えるコマンドの一覧を表示 | 困ったらまずこれ | 自作スキルや拡張のコマンドも一覧に出る |
| `/clear` | 会話をリセットして新しく始める | 話題が完全に変わる時 | 前の会話は /resume で戻れる |
| `/compact [指示]` | 会話を要約して文脈を圧縮 | 長くなって応答が鈍い時 | 例：/compact Dockerの設定変更の経緯を残して |
| `/resume [名前]` | 前のセッションを再開 | 昨日の続きをしたい時 | /continue でも可。引数なしで一覧から選べる |
| `/rewind` | コードと会話を前の時点に戻す | 変更が思った方向と違った時 | 取り消しの安全網 |
| `/exit` | Claude Codeを終了 | 作業終了 | /quit でも可。Ctrl+D でも終了 |

#### 17-2-2. 文脈・コスト・モデル

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/context` | 文脈（コンテキスト）の使用状況を確認 | どれだけ埋まっているか見たい時 | /compact や /clear の判断材料 |
| `/usage` | 使用量の確認 | 上限・コストが気になる時 | 旧バージョンでは /cost の場合あり |
| `/model [名前]` | 使うモデルを切り替え | 軽い作業は軽量モデルで | 例：/model sonnet |
| `/effort` | 思考の深さを調整 | 簡単な作業は low で節約 | 利用できるモデルで異なる |

#### 17-2-3. 設定・プロジェクト

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/init` | プロジェクト用の CLAUDE.md を生成 | 新しいプロジェクトの最初に | 生成後に自分のルールを追記する |
| `/memory` | CLAUDE.md やメモリを編集 | ルールを足したい時 | プロジェクト共通の決まりをここに書く |
| `/config` | 設定を開く | テーマ・動作の変更 | — |
| `/permissions` | 許可／禁止するコマンドの管理 | 毎回の確認を減らしたい／危険な操作を禁止したい | docker rm や prune は許可に入れない方が安全 |
| `/status` | バージョン・モデル・アカウント情報 | 環境の確認 | 問い合わせ時にも使う |
| `/doctor` | インストールの診断 | 動かない・挙動が変な時 | WSL・Node.js周りの問題の切り分けに |
| `/login／/logout` | ログイン・ログアウト | アカウント切替 | — |
| `/add-dir` | 作業対象のフォルダを追加 | 別フォルダのファイルも扱いたい時 | 利用可否はバージョンで異なる |
| `/terminal-setup` | ターミナルのキー設定（Shift+Enter等） | 改行キーが効かない時 | 対応ターミナルのみ |
| `/export` | 会話の書き出し | 記録・共有したい時 | バージョンで有無あり |
| `/release-notes` | 更新内容の確認 | 新機能を知りたい時 | バージョンで有無あり |

#### 17-2-4. 拡張（スキル・MCP・エージェント）

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/mcp` | MCPサーバー（外部連携）の管理 | 外部ツールと繋ぎたい時 | ターミナルからは claude mcp add でも追加できる |
| `/plugin` | プラグインの管理 | スキル等の詰め合わせを導入 | — |
| `/agents` | サブエージェントの管理 | 役割別のエージェントを作る | 定義は .claude/agents/ に置かれる |
| `/hooks` | フック（自動実行）の設定 | 保存時に自動でテスト等 | — |
| `/スキル名` | 自作スキルの呼び出し | 作ったスキルを手動で使う | スキル名＝フォルダ名。例：/my-skill。/help の一覧で確認 |

#### 17-2-5. 計画・品質・同梱スキル

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/plan` | 計画モードに入る | いきなり変更せず方針を確認したい時 | 変更の前に計画を出させる |
| `/code-review` | 現在の差分をレビュー | コミット前 | オプションはバージョンで異なる |
| `/simplify` | コードの品質確認・簡潔化 | リファクタ時 | 同梱スキルのひとつ |
| `/debug` | デバッグ用の同梱スキル | エラー調査 | — |
| `/loop` | 繰り返し実行 | 定期チェック | 間隔を省くとClaudeが自分で調整 |
| `/batch` | まとめて処理する同梱スキル | 同じ変更を多数に | — |
| `/claude-api` | Claude API の参照資料を読み込む | APIを使うコードを書く時 | — |
| `/security-review` | 変更の脆弱性チェック | 公開・リリースの前 | バージョンで有無あり |
| `/goal [条件\|clear]` | 達成条件を設定して続けさせる | 長めの作業を任せる時 | /goal clear で解除 |

#### 17-2-6. その他

| コマンド | 意味 | 使う場面 | 補足 |
|---|---|---|---|
| `/insights` | セッションを分析したレポートを生成 | 使い方の振り返り | — |
| `/desktop` | セッションをデスクトップアプリで続ける | GUIで続けたい時 | macOS／Windows |
| `/feedback` | フィードバックを送る | 不具合報告 | — |
| `/advisor [モデル\|off]` | 助言役のモデル設定 | 複雑な判断をさせたい時 | 環境依存。/help で確認 |
| `/teleport` | Claude Code on the web のセッションを手元に持ってくる | Web版から手元に移す時 | — |
| `/web-setup` | GitHubを接続（Web版） | Web版の初期設定 | — |

### 17-3. 起動・CLI・キー操作・Docker作業での使い方

#### 17-3-1. インストール・起動

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| インストール（npm版） | `npm install -g @anthropic-ai/claude-code` | Node.js が必要。要件・他の導入方法は公式ドキュメントで確認 |
| 起動（対話モード） | `claude` | プロジェクトのフォルダで実行する |
| 最初の質問つきで起動 | `claude "このプロジェクトの構成を説明して"` |  |
| バージョン確認 | `claude --version` |  |
| 更新 | `claude update` |  |
| 直前の会話を続ける | `claude -c` | --continue と同じ |
| 名前／IDで再開 | `claude -r "名前"` | --resume と同じ |
| モデルを指定して起動 | `claude --model sonnet` | 別名：sonnet／opus など |
| MCPサーバーを追加 | `claude mcp add ...` | 引数は公式ドキュメントで確認 |

#### 17-3-2. 非対話（スクリプト）

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| 1回だけ質問して結果を出力 | `claude -p "質問"` | -p＝print mode。スクリプト・パイプ向け |
| Dockerのログを渡して原因を聞く | `docker logs web \| claude -p "エラーの原因を3行で"` | bash・PowerShellどちらでも使える |
| ファイルを渡して要約 | `Get-Content app.log \| claude -p "要約して"` | PowerShell版。bashは cat app.log \| claude -p ... |
| 出力形式を指定 | `claude -p "要約して" --output-format json` | json／text など。スクリプトで結果を処理する時 |
| ターン数の上限 | `claude -p "〜" --max-turns 3` | 暴走防止 |
| 続きの会話を非対話で | `claude -c -p "続きをお願い"` |  |

#### 17-3-3. 入力のコツ

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| ファイルを指定して話す | `@Dockerfile を見て改善案を出して` | @ のあとにファイル名（補完が出る） |
| シェルコマンドを直接実行 | `!docker ps -a` | ! で始めるとセッション内でそのまま実行され、出力が会話に入る |
| スラッシュコマンド／スキル | `/ を入力` | 一覧が出る。続けて文字で絞り込み |
| 改行 | `Shift+Enter（または \ のあと Enter）` | ターミナルによって異なる。効かない時は後者 |

#### 17-3-4. キー操作

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| 実行中の生成を止める | `Esc` | Ctrl+C でも中断できる |
| 前の状態に戻す | `Esc を2回（または /rewind）` | バージョンで挙動が違う場合あり |
| 権限モードの切替（計画モード等） | `Shift+Tab` | 作業の進め方（確認／自動／計画）を切り替える |
| 画面クリア | `Ctrl+L` | 会話は消えない（/clear とは別） |
| 終了 | `Ctrl+D（または /exit）` |  |
| モデル切替 | `Alt+P（Macは Option+P）` | /model でも可 |
| 直前の入力を編集 | `↑ キー` | 履歴の呼び出し |
| メモリへの追記 | `/memory` | CLAUDE.md を開いて編集 |

#### 17-3-5. Docker作業での使い方

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| ログの原因を調べてもらう | `docker compose logs の最後100行を見て、エラー原因と対処を教えて` | まず !docker compose logs --tail 100 で会話に出力してから聞くと確実 |
| Dockerfileをレビューしてもらう | `@Dockerfile をレビューして。イメージを小さくする案を出して` | 変更は差分を確認してから承認する |
| compose.yaml を直してもらう | `@compose.yaml に Postgres を追加して。変更前に差分を見せて` | インデント（スペース2つ）の間違いも指摘してもらえる |
| 容量を整理する相談 | `!docker system df の結果を見て、安全に消せる物と消してはいけない物を分けて` | ⚠ prune／rm は実行前にコマンドの中身を必ず読む |
| 安全のための設定 | `/permissions で docker rm／prune を許可リストに入れない` | 毎回確認が出る状態にしておくのが新人向けには安全 |
| 出力をそのまま渡す | `docker compose ps \| claude -p "落ちているサービスと原因候補を教えて"` | bash／PowerShell共通 |
| 差分レビュー | `git diff \| claude -p "この変更のリスクを3つ"` | コミット前の確認に |
| コマンド案を相談 | `docker run のオプションをこの要件で組み立てて。まず説明だけして実行はしないで` | ⚠ 実行させる前に内容を確認する習慣を |

#### 17-3-6. 権限・安全

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| 使えるツールを限定 | `claude --allowedTools "Read" "Grep"` | 読み取り専用で使う時。名称・書式は claude --help で確認 |
| 使わせないツールを指定 | `claude --disallowedTools "Bash(docker rm:*)"` | 書式はバージョンで変わる。/permissions の方が確実 |
| 計画モードで起動 | `claude --permission-mode plan` | 変更せずに計画だけ出す |
| ⚠ 全許可で起動 | `claude --dangerously-skip-permissions` | ⚠ 確認なしで実行される。新人・本番環境では使わない |

#### 17-3-7. 起動オプション

| 操作・やりたいこと | 入力 | 補足 |
|---|---|---|
| 作業フォルダを追加 | `claude --add-dir ../other` | 複数フォルダをまたぐ時 |
| システムプロンプトに追記 | `claude --append-system-prompt "回答は日本語で"` | 毎回の指示を足す |
| MCP設定を指定 | `claude --mcp-config mcp.json` |  |
| 詳細ログ／デバッグ | `claude --verbose ／ claude --debug` | 動作調査用 |
| ヘルプ | `claude --help` | 最新のオプション一覧はここで確認 |

### 17-4. 設定ファイル・フォルダの置き場所

| 名前 | 場所（プロジェクト） | 場所（個人） | 役割 | 補足 |
|---|---|---|---|---|
| CLAUDE.md | `プロジェクト直下 ./CLAUDE.md` | `~/.claude/CLAUDE.md` | 毎回読み込まれる指示・ルール書 | /init で雛形を作成。チーム共通ルールをここに書く |
| スキル | `./.claude/skills/<名前>/SKILL.md` | `~/.claude/skills/<名前>/SKILL.md` | 自作スキル本体（説明＋手順＋同梱ファイル） | フォルダ名がスラッシュコマンド名になる。description がスキルの発動条件になる |
| カスタムコマンド（旧方式） | `./.claude/commands/<名前>.md` | `~/.claude/commands/<名前>.md` | Markdown1枚で作るコマンド | 今も動くが、スキル方式が推奨。作ると同名の /コマンドになる |
| サブエージェント | `./.claude/agents/<名前>.md` | `~/.claude/agents/<名前>.md` | 役割別の専用エージェント定義 | /agents から作成・管理できる |
| 設定（共有） | `./.claude/settings.json` | `~/.claude/settings.json` | 権限・フック・モデル等の設定 | Gitで共有する設定 |
| 設定（個人・非共有） | `./.claude/settings.local.json` | `—` | 自分だけの設定 | Gitに入れない |
| MCP設定 | `./.mcp.json` | `—` | プロジェクトのMCPサーバー定義 | /mcp または claude mcp add で管理 |
| WSLとWindowsの違い | `WSL内のプロジェクト：~/... を使用` | `WSL：/home/ユーザー名/.claude／Windows：C:\Users\ユーザー名\.claude` | WSLのClaude CodeとWindowsのClaude Codeは別フォルダ | スキルは使う側（WSL or Windows）の .claude/skills に置く。作業は ~/ 配下が高速 |

## 18. 作ったスキルまとめテンプレート

自作スキルが増えてきたら、**台帳（一覧表）で管理し、1枚のカードで紹介** できるようにしておくと、棚卸し・紹介記事・依頼時の説明がすべて楽になります。

### 18-1. 使い方の流れ

| STEP | やること | 使うもの |
|---|---|---|
| 1 | スキルのフォルダから、名前・説明を自動で一覧にする | 18-4 のコマンド／スクリプト |
| 2 | 台帳（スプレッドシートの「スキル台帳」シート）に貼り付けて、カテゴリ・対象・根拠などを手で追記 | 18-2 の列 |
| 3 | 紹介したいスキルは、1枚のカードにまとめる | 18-3 のカード |
| 4 | 新しく作る時は、ひな形から始める | 18-5 の SKILL.md ひな形 |

### 18-2. スキル台帳の列（スプレッドシートの「スキル台帳」シート）

| 列名 | 書くこと |
|---|---|
| スキル名（フォルダ名） | スラッシュコマンド名と同じ。英小文字とハイフン推奨 |
| カテゴリ | 研修設計／資料作成／開発支援／文章／デザイン など自分で決めた分類 |
| 一言説明 | 何ができるか1行 |
| 対象者 | 誰のためのスキルか |
| 発動ワード | description に書いた「こう頼まれたら使う」のキーワード |
| 入力／出力形式 | 何を渡すと何（PPT／Word／Markdown／Excel）ができるか |
| 根拠・参照 | 論文・公式ドキュメント。出典と限界を書く |
| 同梱ファイル | scripts／templates など |
| 保存場所／作成日／最終更新 | どの .claude/skills か、いつ作って、いつ直したか |
| 状態 | 使用中／改善中／試作／廃止 |
| 関連スキル／使い方メモ | セットで使うスキル、気づき |

### 18-3. スキル紹介カード（1スキル1枚）

```markdown
# 🧰 スキル紹介カード

| 項目 | 内容 |
|---|---|
| スキル名 | my-skill-name |
| 一言で言うと | 〇〇を△△にするスキル |
| こんな時に使う | ・場面1 ／ ・場面2 |
| 呼び出し方 | /my-skill-name ／「〇〇して」と話しかける |
| 入力 | テーマ・対象・時間 など |
| 出力 | PPT／Word／Markdown／Excel |
| 根拠・参照 | 論文・公式ドキュメント（出典を明記） |
| 工夫したところ | 自動チェック・ひな形 など |
| 注意点・限界 | やらないこと／要確認事項 |
| 関連スキル | 〇〇 ／ △△ |
| 状態／更新日 | 使用中／2026-10-23 |

```

### 18-4. スキル一覧を自動で出すコマンド

| 環境 | やりたいこと | コマンド | 補足 |
|---|---|---|---|
| Ubuntu（bash） | スキルのフォルダ一覧 | `ls ~/.claude/skills` |  |
| Ubuntu（bash） | name と description を一覧 | `grep -H -E "^(name\|description):" ~/.claude/skills/*/SKILL.md` | 複数行の description は先頭行のみ表示される |
| Ubuntu（bash） | 台帳CSVを作る | `python3 skills_to_csv.py` | skills_ledger.csv ができる → スプレッドシートに貼り付け／インポート |
| PowerShell（Windows側のスキル） | スキルのフォルダ一覧 | `Get-ChildItem $HOME\.claude\skills -Directory` |  |
| PowerShell（Windows側のスキル） | name と description を一覧 | `Select-String -Path "$HOME\.claude\skills\*\SKILL.md" -Pattern '^(name\|description):'` |  |
| PowerShell（Windows側のスキル） | 台帳CSVを作る | `python skills_to_csv.py "$HOME\.claude\skills\"` | Pythonが入っている場合 |

**skills_to_csv.py**（Python 3。フォルダ名・name・description・ファイル数・最終更新を台帳用CSVにします）

```python
# skills_to_csv.py ー ~/.claude/skills 内の SKILL.md から台帳用CSVを作る
import csv, re, sys, datetime
from pathlib import Path

def read_front(path):
    text = path.read_text(encoding="utf-8", errors="replace")
    m = re.match(r"^---\s*\n(.*?)\n---", text, re.S)
    d, key = {}, None
    if m:
        for line in m.group(1).splitlines():
            kv = re.match(r"^([A-Za-z_-]+):\s*(.*)$", line)
            if kv:
                key = kv.group(1)
                d[key] = kv.group(2).strip().strip('"')
            elif key and line.strip():
                d[key] += " " + line.strip()
    return {k: v.strip().strip('"').strip() for k, v in d.items()}

roots = [Path(p).expanduser() for p in (sys.argv[1:] or ["~/.claude/skills"])]
rows = []
for root in roots:
    for f in sorted(root.glob("*/SKILL.md")):
        d = read_front(f)
        files = sum(1 for _ in f.parent.rglob("*") if _.is_file())
        mtime = datetime.date.fromtimestamp(f.stat().st_mtime).isoformat()
        rows.append([f.parent.name, d.get("name", ""), d.get("description", ""),
                     files, mtime, str(f.parent)])
with open("skills_ledger.csv", "w", newline="", encoding="utf-8-sig") as fp:
    w = csv.writer(fp)
    w.writerow(["フォルダ名", "name", "description", "ファイル数", "最終更新", "保存場所"])
    w.writerows(rows)
print(len(rows), "skills -> skills_ledger.csv")

```

> 作成された `skills_ledger.csv` は Excel／Googleスプレッドシートで開けます（日本語が文字化けしないよう UTF-8 BOM付きで出力）。

### 18-5. SKILL.md ひな形

`~/.claude/skills/スキル名/SKILL.md` として保存します（フォルダ名がスラッシュコマンド名になります）。

```markdown
---
name: my-skill-name
description: 〜するスキル。「〇〇して」「△△を作って」と頼まれた時に使う（発動条件を具体的に書く）
---

# スキル名

## 目的
このスキルで何を実現するか（1〜2行）

## 使う場面
- こういう依頼の時
- こういう入力がある時

## 手順
1. ヒアリング（不足情報だけを質問）
2. 作成（テンプレート／ヘルパースクリプトを使う）
3. 検査（チェック項目で確認）
4. 納品（ファイルを出力して要点を報告）

## 出力形式
PPT／Word／Markdown／Excel のどれか

## 同梱ファイル
- scripts/ …… 自動チェックなどのスクリプト
- templates/ …… ひな形

## 注意・やらないこと
- 根拠のない断定をしない（出典と限界を明記）

```

### 18-6. スキルの品質チェックリスト

| 確認項目 | OK? |
|---|---|
| description に「いつ使うか」（発動条件・キーワード）が具体的に書かれている | ☐ |
| 1スキル1目的になっている（盛り込みすぎていない） | ☐ |
| 手順が「ヒアリング → 作成 → 検査 → 納品」のように順番になっている | ☐ |
| 出典と限界（言い切れないこと）を書く方針が入っている | ☐ |
| 同梱スクリプトは実際に動かして確認した | ☐ |
| パス・パスワード・個人情報など、書いてはいけないものが入っていない | ☐ |
| 台帳とカードを更新した（最終更新日・状態） | ☐ |

## 19. 最後に

- エラーが出たら **最後の行から読む**（原因は大抵そこにあります）
- 分からない時は `docker コマンド --help`
- 壊しても大丈夫。コンテナは **作り直せる** のがDockerの良いところです 🐰

---

© うさうさ研修工房　面白きこともなき世を面白く
