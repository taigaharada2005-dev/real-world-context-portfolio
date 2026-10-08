# Real-world Context

### AIによる現場観察・ヒアリング・改善提案

**AIが現場を観察し、関係者へのヒアリングで背景や制約を理解し、根拠のある改善提案につなげる仕組みを開発しています。**

このポートフォリオでは、そのための記録デバイス **Real-world-context** と、観察・分析・提案を支えるAI基盤 **Real-world-context-os** の2プロジェクトを紹介します。

電子回路・筐体設計から、動画解析、AIモデルの連携、実験・評価まで、現場理解のためのシステムを横断的に開発しています。

> 概要のみを公開しています。ソースコード、詳細な回路・基板・CADデータ、元プロジェクトのGit履歴、映像・音声、実験の生データは含みません。

## 目指す体験

現場の映像だけでは、作業の理由、担当者の判断、品質・安全の条件、例外対応までは分かりません。観察から仮説を立て、不足している情報を質問で確かめ、現場の条件に合う提案へ進むことを目指しています。

```text
現場の記録・観察
      ↓
事実と仮説を整理し、不足情報を特定
      ↓
関係者へのヒアリングで背景・制約を確認
      ↓
確認結果を踏まえた改善提案
```

例えば「新人に任せられる業務を増やしたい」という相談では、作業の流れを観察し、担当者の判断が必要な場面や例外を確認する質問を作り、標準化・教育・役割分担の改善を検討します。機材導入の相談では、購入を前提にせず、負担の原因や作業量を確認するところから始めます。これらは想定する利用例です。

## 2つのプロジェクト

| プロジェクト | システム内の役割 | 主な技術 |
| --- | --- | --- |
| **Real-world-context** | 現実の状況を記録するデバイス | ESP32S3、IMU、GNSS、KiCad、Autodesk Fusion |
| **Real-world-context-os** | 映像の観察、根拠の整理、改善候補・確認質問の生成 | Python、PyAV/FFmpeg、OpenCV、VLM、Pydantic、YOLO、OSNet |

デバイスとAI基盤を一体運用し、観察から対話・提案までをつなぐ全体構成は開発中です。

## Real-world-context — 現場記録デバイス

映像・音声・動き・位置を記録する小型デバイスを設計しています。回路だけでなく、電源、配線、放熱、部品固定、組み立て、整備性を合わせて検討しています。

### 開発内容

- ESP32S3を中心に、カメラ・マイク・microSD・IMU・GNSSを組み合わせる構成の設計。
- KiCadによる回路・基板設計、ERC/DRC、回路と基板の整合性確認。
- Autodesk Fusionによる筐体、部品配置、配線経路、電池・放熱部品のモデル化。
- 部品の挿抜、基板の取り外し、ねじ工具のアクセスを含む組み立て・整備性の検討。
- Pythonの検証スクリプトとレビュー記録による、設計版と出力資料の対応管理。

### 技術的な工夫

**完成状態で収まることに加え、組み立てられることを確認する。** 挿入・取り外し途中の姿勢や工具の動線を検査し、静的な干渉確認だけでは見つからない問題を扱っています。

**電子回路と機構を合わせて見直す。** ケーブルや放熱部品の変更が基板・配線へ及ぼす影響を追跡し、双方の条件が成立する設計を検討しています。

### 現在の状態

マイコンへの書き込み・USBシリアルとIMUの連続取得を確認した記録があり、基板・筐体の設計資料とモデル上の組み立て性レビューを作成しています。実部品の公差、放熱、RF、電源、製造性などの検証は継続中です。完成機の統合動作・長時間記録や量産を達成した段階ではありません。

## Real-world-context-os — 現場理解と提案のAI基盤

映像から時刻付きの観察を得て、改善候補、仮説、別の説明、不足情報、確認質問を整理するPython基盤を開発しています。質問に対応する映像の検索・検証と、現場改善に向けた分析実験の両方に取り組んでいます。

### 開発内容

- 動画の時刻付きフレーム抽出、粗い観察から詳細な確認へ進む段階的な解析。
- 複数の観察役と推論段階を組み合わせ、関連する元動画クリップを選んで確認する構成。
- 改善候補ごとに観察事実・仮説・別の説明・不足情報・確認質問を構造化。
- 仮説に質問を紐づけ、確認する理由と、支持された場合・されない場合の方針を整理。
- 原寸の根拠画像・動画クリップ、HTMLレビュー画面、モデル呼び出し・時間・token・推定費用のログを保存。
- YOLOとOSNetを用いた人物検出・特徴量抽出・短区間の匿名人物照合。
- ローカル実行、Apple Silicon、クラウドGPUでの実験と、モデル構成の比較。

### 技術的な工夫

**観察した事実と推測を分ける。** 根拠の時刻や元画像・クリップを追跡し、映像で分からない点を確認質問として残します。

**不確実さを保ったまま進める。** 画像の不鮮明さ、人物照合の曖昧さ、モデル出力の矛盾を扱い、十分な証拠がない回答は判断保留にします。

**人の評価とAIの推論を分離する。** 実験の採点や目視評価を回答生成へ混ぜず、生成後に別の評価として扱います。将来の現場ヒアリングとは役割を区別しています。

**出力品質と処理費用を一緒に検討する。** 役割分担、画像数、モデル規模、キャッシュ、実行ログを通じて、処理の再現性と予算を管理しています。

### 現在の状態

公開映像の冒頭10分を用い、観察から改善候補・仮説・不足情報・店舗への確認質問を作る実験を実施しています。同じ観察結果を用いた異なる推論モデル構成の比較と、候補ごとの質的レビュー画面も作成しています。

時刻付き動画抽出、証拠付き質問応答、人物検出・特徴量抽出には実装・動作検証の記録があります。一方、実際の関係者との継続的な対話ヒアリング、回答を反映した提案更新、現場での改善効果の実証は今後の検証対象です。モデル比較は提案品質の優劣や正解率を証明するものではありません。

## この開発で示す技術領域

| 領域 | 取り組み |
| --- | --- |
| システム設計 | 現場観察・ヒアリング・改善提案をつなぐ構成の検討 |
| 組み込み・電子回路 | マイコン、センサー接続、電源、基板設計 |
| 機構・CAD | 筐体、部品配置、配線、組み立て・整備性 |
| コンピュータービジョン | 動画処理、人物検出、特徴量、匿名人物照合 |
| AIアプリケーション | VLM連携、構造化出力、仮説・質問生成、証拠管理 |
| 実験・評価 | 回帰テスト、モデル比較、質的レビュー、時間・費用の記録 |

## English summary

**Real-world Context is a development portfolio for an AI system designed to observe on-site activities, conduct interviews, and propose improvements grounded in evidence and operational context.**

The intended workflow moves from observation to hypotheses and information gaps, then to stakeholder interviews and informed recommendations.

- **Real-world-context** is the recording-device project. It covers ESP32S3-based electronics and sensors, PCB design in KiCad, and enclosure design in Autodesk Fusion, with attention to wiring, assembly, service access, and design consistency.
- **Real-world-context-os** is the AI software foundation. It combines timestamped video analysis, visual language models, evidence retrieval, and structured generation of improvement candidates, hypotheses, alternative explanations, and interview questions. Additional work includes person detection, short-window association, review interfaces, and model/runtime/cost comparisons.

Experiments have generated improvement candidates and clarification questions from the first ten minutes of a public video. Live stakeholder interviews, interview-driven proposal updates, integrated device-to-AI operation, and demonstrated operational improvements remain future validation steps.

This repository shares project summaries only. Source code, detailed hardware designs, original development histories, recordings, and raw experimental data remain private.

---

開発中のプロジェクトです。記載内容は2026年10月8日に確認した開発資料に基づき、実装・実験済みの内容と今後の検証対象を区別しています。
