# SpikeVote-project (version alpha1)

## Project Overview / プロジェクト概要

**EN**  
This project aims to build a semi-automated labeling pipeline to support curation after spike sorting of Neuropixels recording data.

As a long-term plan, we are considering collecting curated datasets from multiple laboratories and researchers, integrating them, and training a machine-learning model based on UnitRefine to build a custom shared model for supporting unit classification.

By returning the trained model to the laboratories that contributed training data, we aim to help improve the efficiency and consistency of curation work in each laboratory.

**JP**  
本プロジェクトは、Neuropixels記録データに対するスパイクソーティング後のキュレーション作業を補助する半自動化ラベリングパイプライン構築を目的としています。

構想として、複数の研究室・研究者にキュレーション済みデータの提供をお願いし、それらを統合した上で機械学習モデル（UnitRefine）を学習させ、unit分類を支援するためのカスタム共有モデルを構築することを検討しています。

学習によって得られたモデルを、学習データを提供した各研究室に還元することで、それぞれのキュレーション作業の効率化や判断の一貫性向上に役立てることを目指しています。

![spikevote_pipeline_overview_v2](https://github.com/user-attachments/assets/64b5cbc1-de6b-43fa-a342-e5089e2d4852)

**Figure 1. Project concept / 図1. プロジェクト構想**

---

## Background and Purpose / 背景と目的

**EN**  
High-density Neuropixels probes make it possible to record neural activity from many neurons simultaneously. However, manual curation after spike sorting requires substantial time and effort. Another major challenge is that curation results can vary depending on the experimenter or curator.

In this project, we aim to collect and organize training data for automated or semi-automated curation using [UnitRefine](https://github.com/anoushkajain/UnitRefine), a machine-learning toolbox integrated with [SpikeInterface](https://spikeinterface.readthedocs.io).

Ideally, by collecting and integrating curated datasets from multiple laboratories across different animal species, recording conditions, probe types, and experimental paradigms, we hope to build a shared learning model with broad generalization ability.

The primary goal is to reduce the curation burden in each laboratory. At the same time, we hope this project will also contribute to collaborative research based on shared criteria and improve reproducibility across laboratories.

At present, this project is being developed on an individual, best-effort basis. If you support this concept and are willing to participate as a contributor, your involvement would be very welcome.

Contact: Namima  
Email: namimatomoyuki@gmail.com

**JP**  
高密度プローブNeuropixelsによる記録では、多数のニューロン活動を同時に取得できる一方で、スパイクソーティング後の手動キュレーションには多くの時間と労力を要します。また、担当者ごとの判断の違いによって、キュレーション結果にばらつきが生じやすいことも課題です。

本プロジェクトでは、[UnitRefine](https://github.com/anoushkajain/UnitRefine)（[SpikeInterface](https://spikeinterface.readthedocs.io) に統合された機械学習ツールボックス）を用い、キュレーションの自動化・半自動化に向けた学習データを収集・整備することを目指しています。

理想的には、対象動物、記録条件、使用プローブ、実験種の違いを越えて、複数のラボからキュレーション済みデータを収集・統合することで、汎化性能をもつ共有学習モデルを構築したいと考えています。

これにより、各ラボにおけるキュレーション負担の軽減を第一の目的としつつ、共通基準にもとづく共同研究の促進や再現性の向上にもつなげられればと期待しています。

現時点では個人ベースで進めていく予定ですが、もし本構想に賛同し、Contributorsとしてコミットいただける有志の方がいれば、ぜひご参画いただければ幸いです。

管理者の連絡先：波間  
Email: namimatomoyuki@gmail.com

---

## How to Contribute / コントリビューション方法

**EN**  
We welcome contributions from as many experimenters and laboratories as possible.

1. After running Kilosort4 and completing manual curation in Phy, please run SpikeVote on the curated files to export training data.
2. Please push the exported CSV parameter file and JSON metadata file to the `data` directory directly under the GitHub repository.

**JP**  
できる限り多様な実験者からの貢献を歓迎します。

1. Kilosort4後、Phyによるmanualキュレーション実行済みファイルに対して、SpikeVoteを実行してください（学習データの提供）。
2. パラメータファイルのCSVファイルとメタファイルのJSONファイルを、GitHubディレクトリ直下の`data`ディレクトリにpushしてください。

---

## Development Policy / 開発方針

**EN**  

- Distribute a Dockerfile for running automated spike sorting with Kilosort4, drift correction with SpikeInterface, and waveform quality metric calculation with SpikeInterface in a unified environment.
- Integrate and share output results.
- Allow curator information to be manually entered into a metadata file when running the functions, so that the data can be filtered or fine-tuned during model training.

**JP**  

- オートソーティング（Kilosort4）、ドリフトコレクション（SpikeInterface）、波形品質メトリクス計算（SpikeInterface）を一括実施するためのDockerfileを配布します。
- 出力結果を統合・シェアします。
- 関数実行時にメタファイルにcurator情報を手入力することで、モデル学習時にデータフィルタリングやファインチューニングができるように整備します。

---

## Strategy / 戦略

**EN**  
The initial strategy is to train a classifier for the following unit categories using UnitRefine:

- SUA
- MUA
- noise

Units that require merging or splitting will be treated separately from these three main categories.

**JP**  
UnitRefineを用いて、以下の分類を学習することを想定しています。

- SUA
- MUA
- noise

mergeもしくはsplitが必要になるようなunitは、上記の3カテゴリ外として扱う方針です。

![spikevote_id_mapping](https://github.com/user-attachments/assets/018c94ea-3f62-4731-9ae9-ed014c4f985e)

---

## Providing Training Data / 学習データの提供について

**EN**  
If you have Neuropixels data that have already been curated in Phy, you only need to run the following notebook once and send the exported CSV and JSON files.

You do not need to send raw data such as `.bin` files.

```text
export_spikedata_curatedunits2v.ipynb
```

The Docker environment for running Kilosort4 and SpikeInterface before executing `export_spikedata_curatedunits2v.ipynb` is available from the SpikeVote repository.

Before running the notebook, please edit only the first configuration cell to match your own dataset and environment.

For details, please refer to `export_spikedata_curatedunits2v.ipynb`.

**JP**  
Phyでキュレーション済みのNeuropixelsデータがあれば、以下のノートブックを1回実行して出力されたCSVとJSONを送るだけです。

生データ（`.bin` 等）を送る必要はありません。

```text
export_spikedata_curatedunits2v.ipynb
```

`export_spikedata_curatedunits2v.ipynb` 実行までの、Kilosort4とSpikeInterfaceの実行環境構築のためのDockerは、SpikeVoteリポジトリから利用できます。

実行前に、スクリプト冒頭の1セルの内容をカスタムに書き換えてから実行してください。

詳細は `export_spikedata_curatedunits2v.ipynb` を参照してください。

---

## Return to Contributors / リターン

**EN**  
Our current plan is to make the UnitRefine-compatible model trained on data from multiple laboratories available to those who contributed training data.

Because this project is currently being developed on an individual, best-effort basis, it may take some time. Thank you for your understanding.

The more people who help with model development and improvement, the faster we will be able to return a useful model to contributors. If you are willing to help, please contact Namima by DM.

**JP**  
現時点の方針として、学習データを提供いただいた方に対して、複数のラボから提供いただいたデータを用いて学習済みのUnitRefine用モデルを使えるようにしたいと考えています。

個人ベースで進めているため時間がかかる予定ですので、その点ご理解いただけますと幸いです。

手伝っていただける人が増えるほど早めにリターンできますので、モデル開発・改善に協力いただける有志の方は、なみままでDMください。

![spikevote_motivation_v2](https://github.com/user-attachments/assets/bf114702-2bb1-4d5e-9be5-4f8da8d36ae3)

---

## Issues and Questions / 問題報告

**EN**  
For bugs, questions, or suggestions, please open an Issue on GitHub.

**JP**  
バグ・質問・提案はGitHubのIssueを開いてください。

---

## Note on the Use of Generative AI / 生成AI利用に関する注意

**EN**  
Please note that generative AI tools, including Claude.ai, are used during coding and development. If you are uncomfortable with this, please refrain from using the project.

**JP**  
注意：コーディングに生成AI（Claude.ai）を利用して進めていますので、抵抗ある方は使用をお控えください。

---

## Maintainer / 管理者

**EN**  
Tomoyuki Namima  
Email: namimatomoyuki@gmail.com

For questions or contact, please open an Issue or contact Namima directly.

**JP**  
なみま  
Email: namimatomoyuki@gmail.com

質問・連絡はIssueまたはなみままで直接お問い合わせください。

---

## Changelog / 開発履歴

| Date / 日付 | Changes / 内容 |
|---|---|
| 2026.3 | Development started (TN). Prototype distribution protocol created. / 開発開始（TN）。配布プロトコルのプロトタイプ作成。 |
