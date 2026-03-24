# Docker 環境構築手順

SpikeVote の実行に必要な Kilosort4 + SpikeInterface 環境を Docker で構築する手順です。

---

## 前提条件

- Docker Desktop がインストール済みであること
  - [Windows / Mac](https://www.docker.com/products/docker-desktop/)
  - Linux: `sudo apt install docker.io`
- Git がインストール済みであること
  - [https://git-scm.com/install](https://git-scm.com/install)
- （推奨）NVIDIA GPU + CUDA ドライバ（Kilosort4 の高速実行に必要）

---

## Step 1: リポジトリをクローンする

```bash
git clone https://github.com/your-username/SpikeVote.git
cd SpikeVote
```

---

## Step 2: Docker イメージをビルドする

```bash
docker build -t spikevote .
```

初回は数分かかります。

---

## Step 3: コンテナを起動する

データフォルダをコンテナにマウントして起動します。

**Windows（PowerShell）:**
```powershell
docker run -it --rm `
  -v "D:\your_data_folder:/workspace/data" `
  -p 8888:8888 `
  spikevote
```

**Mac / Linux:**
```bash
docker run -it --rm \
  -v "/path/to/your_data_folder:/workspace/data" \
  -p 8888:8888 \
  spikevote
```

GPU を使う場合（NVIDIA）:
```bash
docker run -it --rm --gpus all \
  -v "/path/to/your_data_folder:/workspace/data" \
  -p 8888:8888 \
  spikevote
```

---

## Step 4: Jupyter Notebook を開く

コンテナ起動後、ターミナルに表示される URL をブラウザで開きます。

```
http://127.0.0.1:8888/lab?token=xxxxxxxx
```

`/workspace/notebooks/` に `export_unitrefine_data.ipynb` があります。

---

## Step 5: ノートブックを実行する

1. `export_unitrefine_data.ipynb` を開く
2. 冒頭の設定セルを自ラボの環境に書き換える

```python
LAB_ID         = "your_lab_name"
BASE_FOLDER    = "/workspace/data"
SESSION_FOLDER = "subject_id/session_id"
```

3. 上から順番にセルを実行する
4. 完了後、以下の2ファイルが出力される

```
/workspace/data/.../unitrefine_export/
    your_lab_20250101_1200_metrics.csv   ← 送付するファイル
    your_lab_20250101_1200_meta.json     ← 送付するファイル
```

---

## トラブルシューティング

**Docker が起動しない（Windows）**
→ Docker Desktop が起動しているか確認してください。

**マウントしたデータが見えない**
→ Docker Desktop の Settings → Resources → File Sharing に
データフォルダのドライブ（例: `D:\`）が追加されているか確認してください。

**GPU が認識されない**
→ [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) のインストールを確認してください。
GPU なしでも CPU で実行できますが、Kilosort4 は遅くなります。

**ポート 8888 が使用中**
→ `-p 8889:8888` のように別のポートに変更してください。

---

## 注意事項

- `/workspace/data` にマウントしたフォルダ内のデータは読み書きされます。
- コンテナを終了しても `/workspace/data` 内のファイルはローカルに残ります。
- コンテナ内で作成したファイルは `/workspace/data` 以外は終了時に消えます。
