# Real-world Context
## AI現場調査・改善コンサルティングシステム

**産業現場の改善・自動化において、人間による現場調査・分析・コンサルティングがボトルネックとなっている問題を、AIによって解消することを目指しています。**

本ポートフォリオでは、現場の情報を集める記録デバイス **Real-world-context** と、観察・分析・ヒアリング・改善提案を支えるAI基盤 **Real-world-context-os** の開発概要を紹介します。

## 1. プロジェクトの目的

ロボットアームや自動化設備など、産業現場を改善する技術は数多く存在します。しかし、どの技術をどこに導入すべきかを判断するには、現場を調査し、問題を発見し、改善方法を検討する必要があります。この前段階を担う専門家の時間や人数が、改善を進められる規模を左右します。

自動化技術が発展しても、その導入を判断する調査・分析・コンサルティングが人間に依存したままでは、産業全体の改善スピードには限界があります。

本プロジェクトは、このプロセスをAIによって自動化・効率化し、現場改善を大規模に展開できる仕組みの構築を目指します。

## 2. 解決したい課題

### ① 現場データの収集に負担がかかる

作業時間、動作、工程を分析するには、十分な量の観察データが必要です。人間が長時間にわたって作業を観察・記録すると大きな負担がかかり、現場の従業員に任せても追加業務が発生します。そのため、分析に必要なデータを十分に集められない場合があります。

### ② 現場分析が専門家の時間と知識に依存する

収集したデータから課題を特定し、改善策を検討するには専門知識が必要です。専門家の時間や人数には限りがあり、調査できる現場の数や、改善を進めるスピードが制限されます。

### ③ データの収集・分析と改善策の実行が分断されている

データを収集・分析しても、実際の改善につなげるには、設備やソリューションの選定、導入に向けた具体化など、別の知識や支援が必要になります。問題の発見から改善策の選定、実行支援までを一貫して扱う仕組みが求められます。

## 3. 提案するソリューション

**AIによる現場観察・データ収集・分析・課題発見・ヒアリング・改善提案を統合したシステムを開発します。**

### 想定する処理の流れ

1. 現場にカメラ・マイク・各種センサーを設置します。
2. 現場の作業状況を継続的に記録します。
3. AIが映像・音声・センサーデータを分析します。
4. 作業時間、動作、工程、設備の稼働状況などを把握します。
5. 非効率な作業、ボトルネック、安全上のリスクなどの候補を見つけます。
6. 観察だけでは分からない作業の理由や制約を、関係者へのヒアリングで確認します。
7. AIが改善候補を生成し、根拠となるデータとともに提示します。
8. 必要に応じて専門家が検証し、改善策を具体化します。

これはシステム全体の構想です。現在は、記録デバイスの設計と、映像から改善候補・仮説・確認質問を生成するAI基盤を開発・検証しています。作業時間・工程・設備稼働の総合的な把握、対話によるヒアリング、改善策の実行支援は、今後取り組む範囲です。

## 2つのプロジェクトの役割

| プロジェクト | 役割 | 主な技術 |
| --- | --- | --- |
| **Real-world-context** | 現場の映像・音声・センサー情報を記録するデバイスの開発 | ESP32S3、IMU、GNSS、KiCad、Autodesk Fusion |
| **Real-world-context-os** | 映像の分析と、改善候補・仮説・ヒアリング用の質問を生成するAI基盤の開発 | Python、PyAV/FFmpeg、OpenCV、VLM、Pydantic、YOLO、OSNet |

両プロジェクトは共通の目的に向けて開発しています。記録デバイスからAI解析までの接続と、対話を通じた提案の更新は、今後の検証対象です。

## Real-world-context：現場記録デバイス

映像・音声に、動きや位置の情報を組み合わせて記録する小型デバイスを設計しています。限られた筐体内に部品を収めながら、電源、配線、放熱、組み立てやすさ、整備のしやすさを両立させることが課題です。

### 開発しているもの

- **電子回路・基板：** ESP32S3、カメラ、マイク、microSD、IMU、GNSSを組み合わせる構成を設計し、KiCadで回路と基板の整合性を検証。
- **筐体・部品固定：** Autodesk Fusionで部品配置、配線経路、電池や放熱部品の固定方法を検討。
- **設計の検証：** Pythonのスクリプトとレビュー記録で、設計版、検証結果、出力資料の対応を管理。

### 設計で重視していること

完成状態の配置に加え、組み立てや取り外しの途中でも部品が干渉しないかを確認しています。コネクタの挿抜、基板の取り外し、ねじを回す工具の動線まで含めて検討し、実際の作業が成立する設計を目指しています。

また、筐体や放熱部品の変更は基板・配線にも影響します。電子回路と機構の変更を対応付け、両方の条件を満たすように設計を見直しています。

### 現在の到達点

マイコンへの書き込み、USBシリアル通信、IMUの連続取得を確認した記録があります。回路・基板・筐体の設計資料を作成し、CAD上で組み立てや整備の手順を検証しています。

実部品の寸法・公差、放熱、無線性能、電源、製造性の検証は継続中です。完成機での統合動作と長時間記録は、今後確認する段階です。

## Real-world-context-os：現場理解と改善提案のAI基盤

映像から得た観察を、改善候補や確認質問へつなげるPython基盤を開発しています。提案ごとに根拠となる映像を確認できるようにし、観察事実、仮説、別の説明、不足情報を整理して保存します。

### 開発しているもの

- **映像の観察・検索：** 時刻付きのフレームを抽出し、全体の観察から関連区間を絞って詳しく確認する解析処理。
- **改善候補と質問の生成：** 複数のAIモデルや観察役を組み合わせ、仮説ごとに確認する理由と質問を整理する処理。
- **根拠の確認画面：** 元画像や動画クリップと分析結果を対応付け、提案を人が検討できるHTML画面。
- **人物の検出・照合：** YOLOとOSNetを使い、短い時間区間で人物を匿名IDとして対応付ける処理。
- **実験・評価の仕組み：** ローカル環境、Apple Silicon、クラウドGPUでの実行と、モデル構成・処理時間・推定費用の比較。

### 設計で重視していること

**提案の根拠をたどれること。** 観察した時刻と元画像・動画クリップを保存し、どの事実から仮説や質問を作ったかを確認できるようにしています。

**映像だけで分からないことを明確にすること。** 不鮮明な画像、人物照合の曖昧さ、モデル出力の矛盾を扱い、証拠が足りない判断は保留します。現場に確認すべき情報は、ヒアリング用の質問として整理します。

**実験を再検討できること。** モデルへの入力・出力、処理時間、使用量、推定費用を記録します。人による採点や目視評価は推論結果を保存した後に行い、評価情報を回答生成に混ぜない構成にしています。

### 現在の到達点

公開映像の冒頭10分を使い、観察から改善候補、仮説、不足情報、店舗への確認質問を生成する実験を実施しています。同じ観察結果に対して異なる推論モデルを用いる比較と、候補ごとに有効な点や懸念を記録するレビュー画面も作成しました。

時刻付きの映像抽出、根拠付きの質問応答、人物検出・特徴量抽出には実装と動作検証の記録があります。実際の関係者との対話、ヒアリング結果を反映した提案の更新、現場での改善効果の検証は今後の課題です。現在のモデル比較は、提案品質の優劣や正解率を示すものではありません。

## 開発を通じて取り組んでいる技術領域

現場の情報を取得するハードウェアと、その情報を解釈するソフトウェアの両方を扱っています。

| 領域 | 開発内容 |
| --- | --- |
| システム設計 | 現場観察からヒアリング・改善提案へ進む流れの設計 |
| 組み込み・電子回路 | マイコン、センサー接続、電源、回路・基板設計 |
| 機構設計 | 筐体、部品固定、配線、組み立て・整備性の検証 |
| コンピュータービジョン | 動画処理、人物検出、特徴量抽出、匿名人物照合 |
| AIアプリケーション | モデル連携、構造化出力、仮説・質問生成、根拠の管理 |
| 検証・評価 | 回帰テスト、モデル比較、提案の質的評価、時間・費用の記録 |

## 公開範囲

このリポジトリでは、開発の目的、構成、技術的な取り組み、進捗の概要を公開しています。ソースコード、詳細な回路・基板・CADデータ、元プロジェクトのGit履歴、映像・音声、実験の生データは非公開です。

記載内容は2026年10月8日に確認した開発資料に基づいています。

## English summary

**Real-world Context aims to remove the bottleneck created by human-led site investigation, analysis, and consulting in industrial improvement and automation.**

Automation technologies can only be deployed effectively when someone understands the work, identifies the problems, and determines suitable interventions. This project aims to automate and streamline that preparatory process so that improvements can be pursued across more workplaces.

It addresses three challenges: the burden of collecting sufficient operational data, dependence on a limited number of specialists, and the gap between data analysis and the implementation of improvements.

The intended system combines continuous recording, AI observation and analysis, problem identification, stakeholder interviews, and evidence-based recommendations. Specialists can review and refine proposals where needed.

- **Real-world-context:** The recording-device project, covering ESP32S3-based electronics and sensors, PCB design in KiCad, and enclosure design in Autodesk Fusion.
- **Real-world-context-os:** The AI software foundation, covering timestamped video analysis, evidence retrieval, and structured generation of improvement candidates, hypotheses, and interview questions.

Current work includes device design and experiments that generated improvement candidates and clarification questions from the first ten minutes of a public video. Comprehensive operational analysis, live interviews, interview-driven proposal updates, integrated device-to-AI operation, implementation support, and demonstrated improvement outcomes remain future development and validation steps.

This repository publishes project overviews only. Source code, detailed hardware designs, original development histories, recordings, and raw experimental data remain private.
