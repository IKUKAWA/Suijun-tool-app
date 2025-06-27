# 水準測量データ入力アプリ

現場での水準測量作業を効率化するAndroidネイティブアプリです。

## 📱 ダウンロード

**最新版: v1.0.0**

- [APKダウンロード](https://github.com/IKUKAWA/Suijun-tool-app/blob/main/leveling-app-v1.0.0.apk)
- [インストール手順](https://github.com/IKUKAWA/Suijun-tool-app/blob/main/インストール手順.md)
- [使用マニュアル](https://github.com/IKUKAWA/Suijun-tool-app/blob/main/使用方法.md)

## ✨ 主な機能

- 📊 効率的な測点データ入力
- 🧮 往路・復路の自動計算
- 📱 完全オフライン対応
- 📄 CSV形式での成果出力
- 💾 プロジェクト管理
- 🎯 精度管理・閉合差判定

## 📋 概要

水準測量の現場作業を効率化するためのデジタルツールです。測点データの入力、自動計算、結果の可視化、データ管理機能を提供します。

## 🚀 プロジェクト構成

このリポジトリには2つのアプリケーションが含まれています：

### 1. Webアプリケーション (`/project`)
- **技術スタック**: Vite + React + TypeScript + Tailwind CSS
- **データ保存**: LocalStorage
- **対応環境**: モダンWebブラウザ

### 2. Androidアプリケーション (`/android`)
- **技術スタック**: Kotlin + Jetpack Compose + Room Database
- **アーキテクチャ**: MVVM + Clean Architecture
- **対応環境**: Android 7.0 (API Level 24) 以上

## ✨ 主要機能

### プロジェクト管理
- プロジェクトの作成・編集・削除
- プロジェクト情報（名称、日付、天候、作業者、使用機器、起点標高）の管理
- プロジェクト一覧表示と検索

### 測点データ入力
- 往路・復路の測点データ入力
- 測点情報（後視点名、前視点名、距離、後視、前視、備考）の記録
- リアルタイムバリデーション

### 自動計算機能
- 高低差計算（後視 - 前視）
- 標高計算（累積高低差による）
- 往路・復路の平均化計算
- 閉合差計算と許容値判定（√n × 2mm）

### 結果表示
- 計算結果のリアルタイム表示
- 平均化結果テーブル
- 閉合差判定（良好/要確認）の可視化

### データ管理
- CSV形式でのデータエクスポート
- プロジェクトデータの一括管理
- データ統計表示

### データ移行（Android版）
- WebアプリのLocalStorageデータをJSONインポート
- シームレスなデータ移行

## 🛠️ セットアップ

### Webアプリケーション

```bash
# プロジェクトディレクトリに移動
cd project

# 依存関係のインストール
npm install

# 開発サーバーの起動
npm run dev

# プロダクションビルド
npm run build
```

## 🛠️ 技術仕様

- **言語**: Kotlin
- **UI**: Jetpack Compose
- **アーキテクチャ**: MVVM + Clean Architecture
- **データベース**: Room (SQLite)
- **対応OS**: Android 7.0以降

## 📋 システム要件

- Android 7.0 (API Level 24) 以降
- 100MB以上の空き容量
- インターネット接続不要

## 🚀 開発・ビルド

```bash
# リポジトリクローン
git clone https://github.com/IKUKAWA/Suijun-tool-app.git
cd Suijun-tool-app

# キーストア生成
chmod +x scripts/generate_keystore.sh
./scripts/generate_keystore.sh

# リリースビルド
chmod +x scripts/build_release.sh
./scripts/build_release.sh
```

### 開発環境セットアップ

1. Android Studio (Iguana 2023.2.1以降) でプロジェクトを開く
2. `/android` ディレクトリを開く
3. Gradle同期を実行
4. エミュレータまたは実機で実行

## 📱 データ移行手順

WebアプリからAndroidアプリへのデータ移行：

1. **Webアプリでデータをエクスポート**
   - ブラウザの開発者ツール（F12）を開く
   - Consoleタブで以下を実行：
   ```javascript
   console.log(JSON.stringify({
     projects: JSON.parse(localStorage.getItem('surveying_projects') || '[]'),
     stations: JSON.parse(localStorage.getItem('surveying_stations') || '[]')
   }))
   ```
   - 出力されたJSONをコピー

2. **Androidアプリでデータをインポート**
   - データ管理画面を開く
   - インポートボタンをタップ
   - JSONデータを貼り付けて実行

## 📊 データフォーマット

### プロジェクトデータ
```typescript
{
  id: string,
  name: string,
  date: string,
  weather: string,
  operator: string,
  instrument: string,
  baseElevation: number,
  createdAt: string,
  updatedAt: string
}
```

### 測点データ
```typescript
{
  id: string,
  projectId: string,
  backSightPoint: string,
  foreSightPoint: string,
  distance: number | null,
  backSight: number | null,
  foreSight: number | null,
  notes: string | null,
  order: number,
  route: 'outbound' | 'return'
}
```

## 🔧 技術仕様

### Webアプリケーション
- **フレームワーク**: React 18.3
- **ビルドツール**: Vite
- **言語**: TypeScript
- **スタイリング**: Tailwind CSS
- **アイコン**: Lucide React

### Androidアプリケーション
- **言語**: Kotlin
- **UI**: Jetpack Compose + Material 3
- **データベース**: Room (SQLite)
- **依存性注入**: Hilt
- **非同期処理**: Coroutines + Flow
- **最小SDK**: API 24 (Android 7.0)
- **ターゲットSDK**: API 34 (Android 14)

## 📐 計算ロジック

### 高低差計算
```
高低差 = 後視 - 前視
```

### 標高計算
```
標高 = 前測点の標高 + 高低差
```

### 閉合差判定
```
許容閉合差 = √(測点数) × 2mm
判定 = |往路総高低差 + 復路総高低差| ≤ 許容閉合差
```

## 🚧 今後の拡張予定

- [ ] GPS連携による位置情報記録
- [ ] カメラ機能による現場写真添付
- [ ] 音声入力対応
- [ ] クラウド同期機能
- [ ] iOS版の開発（Kotlin Multiplatform Mobile）
- [ ] オフライン対応の強化

## 📄 ライセンス

[ライセンス情報を記載]

## 🤝 貢献

プルリクエストや Issue の報告を歓迎します。

### 開発に参加する

1. このリポジトリをフォーク
2. 機能ブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add some amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

## 📞 サポート

- **Email**: ichikawa@shin-eng.co.jp
- **営業時間**: 平日 9:00-17:00

## 📄 ライセンス

MIT License
