---
layout: default
title: "町田市 市民協働フェスティバル まちカフェ！ 公式案内"
description: "町田の市民活動・NPO・企業・行政が集う年に一度のお祭り「まちカフェ！」のイベント案内ページです。"
---

# ☕ 町田市 市民協働フェスティバル「まちカフェ！」
## 〜 つながる、みつかる、町田の未来 〜

「まちカフェ！」は、町田市内で活動するNPOや市民活動団体、企業、行政が一堂に集まり、日頃の活動を発表したり、体験ワークショップを行ったりする年に一度の大イベントです。

子どもからシニアまで、町田をもっと楽しくしたい人なら誰でも大歓迎！あなたにぴったりの活動や仲間がここで見つかるかもしれません。お気軽にご参加ください！

[案内パンフレット(PDF)をダウンロード]
[町田市公式ページ（詳細）へのリンク]

---

## 📅 開催概要

| 項目 | 内容 |
| :--- | :--- |
| **開催期間** | 2026年11月下旬 〜 12月上旬（※目安。確定日付を入力） |
| **メイン会場** | 町田市役所本庁舎 / 町田市民フォーラム |
| **サテライト会場** | 市内各所の地域センター、オンラインなど |
| **参加費** | 入場無料（一部ワークショップは材料費等実費あり） |
| **主催** | 町田市市民協働フェスティバル「まちカフェ！」実行委員会 / 町田市 |

---

---

## 🎪 出展・イベント団体一覧

今年度のまちカフェ！を盛り上げてくれる魅力的な団体の一覧です。
以下のリンクから全日程・全会場のデータを直接確認するか、このページの下部をご覧ください。

👉 [別ページで一覧を大きく見る場合はこちら](./events.md)

---

{% include_relative events.md %}

---
## 🗺️ アクセス

### メイン会場：町田市役所本庁舎
* 〒194-8520 東京都町田市森野2-2-22
* 小田急線町田駅「西口」から徒歩約8分
* JR横浜線町田駅「北口」から徒歩約11分
> ※イベント当日は混雑が予想されます。公共交通機関でのご来場にご協力をお願いいたします。

---
## 🗺️ 会場フロアマップ

<div class="floor-map-container">
  <div class="tab-buttons" style="margin-bottom: 15px;">
    <button class="tab-btn active" onclick="switchFloor('floor1')">1階 (1F)</button>
    <button class="tab-btn" onclick="switchFloor('floor2')">2階 (2F)</button>
    <button class="tab-btn" onclick="switchFloor('floor3')">3階 (3F)</button>
  </div>

  <div id="floor1" class="tab-content" style="display: block;">
    <img src="1F.jpeg" alt="1階 フロアマップ" style="width: 100%; max-width: 600px; border: 1px solid #ddd; border-radius: 6px;">
  </div>
  
  <div id="floor2" class="tab-content" style="display: none;">
    <img src="2F_floor1.jpg" alt="2階 フロアマップ" style="width: 100%; max-width: 600px; border: 1px solid #ddd; border-radius: 6px;">
  </div>
  
  <div id="floor3" class="tab-content" style="display: none;">
    <img src="FloorMAP3F.jpg" alt="3階 フロアマップ" style="width: 100%; max-width: 600px; border: 1px solid #ddd; border-radius: 6px;">
  </div>
</div>

<style>
  .tab-btn {
    padding: 10px 20px;
    font-size: 14px;
    cursor: pointer;
    background: #f0f0f0;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-right: 5px;
    transition: 0.2s;
  }
  .tab-btn.active {
    background: #007bff;
    color: white;
    border-color: #007bff;
    font-weight: bold;
  }
  .tab-btn:hover {
    opacity: 0.8;
  }
</style>

<script>
  function switchFloor(floorId) {
    // すべてのコンテンツを非表示にする
    var contents = document.getElementsByClassName('tab-content');
    for (var i = 0; i < contents.length; i++) {
      contents[i].style.display = 'none';
    }
    // すべてのボタンの「active」クラスを消す
    var buttons = document.getElementsByClassName('tab-btn');
    for (var i = 0; i < buttons.length; i++) {
      buttons[i].classList.remove('active');
    }
    
    // 選択された階数だけを表示してボタンを青くする
    document.getElementById(floorId).style.display = 'block';
    event.currentTarget.classList.add('active');
  }
</script>

## 🙋 よくある質問（FAQ）

**Q. 事前申し込みは必要ですか？**
A. メイン会場への入場やブース見学は事前申し込み不要です。一部の特別講座や先着順のワークショップについては、事前に各団体へのお申し込みが必要な場合があります。

**Q. 子ども連れでも楽しめますか？**
A. はい！キッズスペースや、お子様向けの体験ブース、クイズラリーなど、小さなお子様から楽しめる企画をたくさんご用意しています。

---

## 🤝 参加団体・ボランティアの皆様へ

まちカフェ！は、市民の皆さんの手で作るイベントです。
当日の運営ボランティアの募集や、出展団体向けのミーティング情報などは、以下のリンクまたはGitHub Issueから随時発信しています。

* [ボランティア応募フォーム（リンク）]
* [出展団体向けマイページ（リンク）]

---

## ✉️ お問い合わせ

**町田市市民協働フェスティバル「まちカフェ！」実行委員会 事務局**
（町田市 市民部 市民協働推進課内）

* TEL：`042-724-4362`（直通）
* FAX：`042-724-4378`
* メール：`mcityXXXX@city.machida.tokyo.jp` （※実際のメールアドレス）
