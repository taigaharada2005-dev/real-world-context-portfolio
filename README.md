# Real-world Context

**AIが現場を観察し、ヒアリングを通じて背景や制約を理解し、改善策を提案するシステムを開発しています。**

本ポートフォリオでは、現場の情報を集める記録デバイス **Real-world-context** と、その情報を分析するAI基盤 **Real-world-context-os** を紹介します。電子回路・筐体設計から動画解析、AIモデルの連携、検証までを扱う開発プロジェクトです。

## 解決したい課題

現場の業務を改善するには、作業の様子に加えて、その作業が必要な理由や、担当者が判断するときの条件を理解する必要があります。映像から動きや手順を観察できても、品質の基準、例外への対応、設備や人員の制約までは読み取れません。

そこで、AIによる観察とヒアリングを組み合わせます。映像から気づいた点を仮説として整理し、関係者に確かめるべき事項を質問にします。その回答を踏まえ、現場の条件に合った改善策を提案することを目指しています。

## システムが目指す流れ

| 段階 | AIの役割 |
| --- | --- |
| **1. 観察** | 現場の記録から、作業の流れや気になる事象を捉える |
| **2. 仮説の整理** | 観察した事実と推測を分け、改善の可能性と不足情報を整理する |
| **3. ヒアリング** | 関係者に質問し、作業の理由、判断基準、制約を確かめる |
| **4. 提案** | 観察とヒアリングの結果を基に、改善策とその根拠を示す |

例えば「新人に任せられる業務を増やしたい」という相談では、作業を観察したうえで、熟練者の判断が必要な場面や例外処理を確認します。その情報を基に、手順の標準化、教育、役割分担の見直しを検討します。これは想定する利用例であり、現場で効果を実証した事例ではありません。

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

**Real-world Context is a system under development that aims to observe on-site activities, interview stakeholders, and propose improvements grounded in evidence and operational context.**

Video can reveal what happens, but understanding why it happens also requires knowledge of decision criteria, exceptions, and practical constraints. The intended workflow combines observation, hypothesis formation, stakeholder interviews, and informed recommendations.

The portfolio comprises two projects:

- **Real-world-context:** A compact recording-device project covering ESP32S3-based electronics and sensors, PCB design in KiCad, and enclosure design in Autodesk Fusion. Development addresses wiring, thermal considerations, assembly, service access, and consistency between electrical and mechanical designs.
- **Real-world-context-os:** A Python AI foundation for timestamped video analysis and the structured generation of improvement candidates, hypotheses, alternative explanations, and interview questions. It also includes evidence review interfaces, person detection and short-window association, and model/runtime/cost comparisons.

Experiments using the first ten minutes of a public video have generated improvement candidates and clarification questions. Live stakeholder interviews, interview-driven proposal updates, integrated device-to-AI operation, and demonstrated operational improvements remain future validation steps.

This repository publishes project overviews only. Source code, detailed hardware designs, original development histories, recordings, and raw experimental data remain private.
