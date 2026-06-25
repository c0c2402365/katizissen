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

## 🗺️ 会場案内・フロアマップナビゲーション

<p>見たいエリア・階数のボタンを押すと、広域地図のピン位置と、市役所の詳細フロアマップ画像が同時に切り替わります！</p>

<div class="map-controls" style="margin-bottom: 20px;">
  <button class="floor-btn active" onclick="switchFloorMap('all', 'all_img')">全エリア</button>
  <button class="floor-btn" onclick="switchFloorMap('1f_out', 'all_img')">1F 屋外マルシェ・広場</button>
  <button class="floor-btn" onclick="switchFloorMap('1f_in', 'floor1_img')">1F 屋内（ワンストップロビー・みんなの広場）</button>
  <button class="floor-btn" onclick="switchFloorMap('2f', 'floor2_img')">2F 会議室・おうえんルーム・キッズスペース</button>
  <button class="floor-btn" onclick="switchFloorMap('3f', 'floor3_img')">3F 会議室・アトリウム・議場</button>
  <button class="floor-btn" onclick="switchFloorMap('hall', 'all_img')">市民ホール</button>
</div>

<div class="map-flex-container" style="display: flex; flex-wrap: wrap; gap: 20px;">
  
  <div class="map-block" style="flex: 1; min-width: 320px;">
    <h3 style="margin-top: 0;">📍 アクセスマップ</h3>
    <div id="event-map" style="width: 100%; height: 400px; border-radius: 8px; border: 1px solid #ccc; z-index: 1;"></div>
  </div>

  <div class="image-block" style="flex: 1; min-width: 320px; text-align: center;">
    <h3 style="margin-top: 0; text-align: left;">🏢 庁舎内レイアウト</h3>
    
    <div id="all_img" class="floor-img-content" style="display: block; padding: 40px 20px; background: #f8f9fa; border: 1px dashed #ccc; border-radius: 8px;">
      <p style="color: #666; margin: 0;">上のボタンから「1F屋内」「2F」「3F」を選択すると、<br>ここに詳細なフロアマップ画像が表示されます。</p>
    </div>

    <div id="floor1_img" class="floor-img-content" style="display: none;">
      <img src="1F.jpeg" alt="町田市役所 1階フロアマップ" style="width: 100%; max-width: 450px; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
    </div>
    
    <div id="floor2_img" class="floor-img-content" style="display: none;">
      <img src="2F_floor1.jpg" alt="町田市役所 2階フロアマップ" style="width: 100%; max-width: 450px; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
    </div>
    
    <div id="floor3_img" class="floor-img-content" style="display: none;">
      <img src="FloorMAP3F.jpg" alt="町田市役所 3階フロアマップ" style="width: 100%; max-width: 450px; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
    </div>
  </div>

</div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const defaultCoords = [35.545199, 139.442838];
  let map;
  let markerGroup;

  // 11月29日のイベントエリア定義
  const pinData = [
    { floor: '1f_out', name: '1F 屋外マルシェ（太陽の村、まなクロ等）', coords: [35.54535, 139.44265], desc: '新鮮野菜販売、草木染め、各NPOの物販など多数出展！' },
    { floor: '1f_out', name: '1F 屋外こもれび広場（焼きそば屋台、キッチンカー）', coords: [35.54540, 139.44285], desc: '法政大学焼きそば、リフトカー体験など。' },
    { floor: '1f_in',  name: '1F みんなの広場（認知症相談、リサイクル、相続相談）', coords: [35.54515, 139.44275], desc: '認知症友の会、福祉機器展、無料法律相談など。' },
    { floor: '1f_in',  name: '1F ワンストップロビー（スタンプラリー、工作、体験）', coords: [35.54510, 139.44295], desc: '謎解きスタンプラリー、ボッチャ体験、防災クイズなど盛りだくさん！' },
    { floor: '2f',     name: '2F 市民協働おうえんルーム（助産師相談、サロン、座談会）', coords: [35.54522, 139.44290], desc: '助産師サークル、スマホ相談、脳疲労ケア体験など。' },
    { floor: '2f',     name: '2F 北側廊下・キッズスペース（駄菓子屋、工作、体験）', coords: [35.54525, 139.44305], desc: 'ミニまちだ駄菓子屋、おしごなりきり、クリスマスツリー工作。' },
    { floor: '2f',     name: '2F 会議室（2-1〜2-4）（マッサージ、マイクラ、演劇）', coords: [35.54530, 139.44298], desc: '視覚障害者マッサージ、マインクラフトで農体験など。' },
    { floor: '3f',     name: '3F 会議室（3-1〜3-3）（出張科学クラブ、ものづくり）', coords: [35.54505, 139.44270], desc: 'ひなた村科学クラブ（炭酸水、色のマジック※要予約）など。' },
    { floor: '3f',     name: '3F アトリウム（きんじょの本棚、リス園、国際交流）', coords: [35.54498, 139.44285], desc: '革小物ワークショップ、手話体験、町田リス園展示など。' },
    { floor: '3f',     name: '3F 議場（いのちの授業、ラテンビッグバンド）', coords: [35.54500, 139.44300], desc: '10:15〜 いのちの授業、13:30〜 マチーダ楽団（要予約）。' },
    { floor: 'hall',   name: '町田市民ホール（ヨガ、ナリワイ大集合、絵本読み聞かせ）', coords: [35.54394, 139.44111], desc: '市役所から徒歩数分。ヨガ、盆踊り、色彩セラピーなど。' }
  ];

  document.addEventListener("DOMContentLoaded", function() {
    map = L.map('event-map').setView(defaultCoords, 17);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);

    markerGroup = L.layerGroup().addTo(map);
    showFloorPins('all');
  });

  // 地図のピンと画像を同時にコントロールする関数
  function switchFloorMap(floorId, imgId) {
    // 1. ボタンのactiveクラス切り替え
    const buttons = document.getElementsByClassName('floor-btn');
    for (let btn of buttons) { btn.classList.remove('active'); }
    event.currentTarget.classList.add('active');

    // 2. 地図ピンの切り替え
    showFloorPins(floorId);

    // 3. フロアマップ画像の切り替え
    const imgContents = document.getElementsByClassName('floor-img-content');
    for (let img of imgContents) { img.style.display = 'none'; }
    document.getElementById(imgId).style.display = 'block';
  }

  function showFloorPins(floorId) {
    markerGroup.clearLayers();
    pinData.forEach(function(pin) {
      if (floorId === 'all' || pin.floor === floorId) {
        const marker = L.marker(pin.coords);
        marker.bindPopup(`<b>${pin.name}</b><br><span style="font-size:12px; color:#555;">${pin.desc}</span>`);
        markerGroup.addLayer(marker);
      }
    });

    if (floorId === 'hall') {
      map.panTo([35.54394, 139.44111]);
    } else {
      map.panTo(defaultCoords);
    }
  }
</script>

<style>
  .floor-btn {
    padding: 8px 14px;
    font-size: 13px;
    cursor: pointer;
    background: #f7f9fa;
    border: 1px solid #ced4da;
    border-radius: 20px;
    margin: 3px;
    transition: all 0.2s;
  }
  .floor-btn.active {
    background: #e67e22;
    color: white;
    border-color: #e67e22;
    font-weight: bold;
  }
  .floor-btn:hover {
    background: #ddd;
  }
</style>
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
