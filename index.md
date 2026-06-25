## 🗺️ 会場案内・フロアマップナビゲーション

<p>見たいエリア・階数のボタンを押すと、アクセスマップのピン位置と、右側の詳細なフロアマップ画像が同時に切り替わります。また、地図上のピンをクリックすると各スペースの出展概要が表示されます。</p>

<div class="map-controls" style="margin-bottom: 20px;">
  <button class="floor-btn active" onclick="switchFloorMap('all', 'all_img')">全エリア</button>
  <button class="floor-btn" onclick="switchFloorMap('1f_out', 'all_img')">1F 屋外マルシェ・広場</button>
  <button class="floor-btn" onclick="switchFloorMap('1f_in', 'floor1_img')">1F 屋内（ワンストップ・みんなの広場）</button>
  <button class="floor-btn" onclick="switchFloorMap('2f', 'floor2_img')">2F 会議室・おうえん・キッズスペース</button>
  <button class="floor-btn" onclick="switchFloorMap('3f', 'floor3_img')">3F 会議室・アトリウム・議場</button>
  <button class="floor-btn" onclick="switchFloorMap('hall', 'all_img')">市民ホール</button>
</div>

<div class="map-flex-container" style="display: flex; flex-wrap: wrap; gap: 20px;">
  
  <div class="map-block" style="flex: 1; min-width: 320px;">
    <h3 style="margin-top: 0;">📍 アクセスマップ</h3>
    <div id="event-map" style="width: 100%; height: 420px; border-radius: 8px; border: 1px solid #ccc; z-index: 1;"></div>
  </div>

  <div class="image-block" style="flex: 1; min-width: 320px; text-align: center;">
    <h3 style="margin-top: 0; text-align: left;">🏢 庁舎内・会場レイアウト</h3>
    
    <div id="all_img" class="floor-img-content" style="display: block; padding: 60px 20px; background: #f8f9fa; border: 1px dashed #ccc; border-radius: 8px;">
      <p style="color: #666; margin: 0; font-size: 14px;">
        上のボタンから<b>「1F屋内」「2F」「3F」</b>を選択すると、<br>
        ここに詳細なフロアマップ（画像）が表示されます。<br><br>
        地図のピンからも連動して部屋の位置を確認できます！
      </p>
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

  // 各部屋・ブースとフロア画像IDの紐付けデータを追加
  const pinData = [
    { floor: '1f_out', img: 'all_img', name: '1F 屋外マルシェ', coords: [35.54535, 139.44265], desc: '【正面入口前・屋外】新鮮野菜販売、草木染め、各NPOの物販・体験など多数出展！' },
    { floor: '1f_out', img: 'all_img', name: '1F 屋外ここもれび広場', coords: [35.54540, 139.44285], desc: '【北側・屋外】法政大学焼きそば屋台、各種キッチンカー、リフトカー体験など。' },
    { floor: '1f_in',  img: 'floor1_img', name: '1F みんなの広場', coords: [35.54515, 139.44275], desc: '【1階右下エリア】認知症悩みごと相談、古本リサイクル、遺産・相続無料相談会など。' },
    { floor: '1f_in',  img: 'floor1_img', name: '1F ワンストップロビー', coords: [35.54510, 139.44295], desc: '【1階中央（案内所・エレベーター周辺）】謎解きスタンプラリー、ボッチャ体験、防災クイズなど。' },
    { floor: '2f',     img: 'floor2_img', name: '2F 市民協働おうえんルーム', coords: [35.54522, 139.44290], desc: '【2階下側（201横）】助産師相談、陽だまりカフェ、スマホ相談、脳疲労ケア体験など。' },
    { floor: '2f',     img: 'floor2_img', name: '2F 北側廊下・キッズスペース・東側廊下', coords: [35.54525, 139.44305], desc: '【2階中央〜右側】ミニまちだ駄菓子屋、おしごとなりきり、クリスマスオーナメント工作、里親制度案内など。' },
    { floor: '2f',     img: 'floor2_img', name: '2F 会議室（2-1 〜 2-4）', coords: [35.54530, 139.44298], desc: '【2階左側・赤色202-204エリア】マッサージ体験、点字、iPhone教室、マイクラ農体験、演劇WS。' },
    { floor: '3f',     img: 'floor3_img', name: '3F 会議室（3-1 〜 3-3）', coords: [35.54505, 139.44270], desc: '【3階上側・302近辺】エンディングウェア発表会、ブラックライトアート空間、ひなた村出張科学クラブなど。' },
    { floor: '3f',     img: 'floor3_img', name: '3F アトリウム', coords: [35.54498, 139.44285], desc: '【3階中央大きな広場】革小物WS、リス園写真展示、手話サークル、やさしい日本語クイズ、無料相談。' },
    { floor: '3f',     img: 'floor3_img', name: '3F 議場', coords: [35.54500, 139.44300], desc: '【3階右側・議場】10:15〜 いのちの授業、13:30〜 マチーダ楽団ラテンジャズ（※要予約）。' },
    { floor: 'hall',   img: 'all_img', name: '町田市民ホール（サテライト会場）', coords: [35.54394, 139.44111], desc: '【市役所から西へ徒歩3分】会議室1〜5にて、ヨガ、ナリワイ大集合、絵本読み聞かせ、盆おどりなど開催！' }
  ];

  document.addEventListener("DOMContentLoaded", function() {
    map = L.map('event-map').setView(defaultCoords, 17);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);

    markerGroup = L.layerGroup().addTo(map);
    showFloorPins('all');
  });

  // ボタンから地図と画像を連動切り替え
  function switchFloorMap(floorId, imgId) {
    const buttons = document.getElementsByClassName('floor-btn');
    for (let btn of buttons) { btn.classList.remove('active'); }
    
    // スクリプトからの自動呼び出しでない（クリックイベントがある）場合のみactiveを付与
    if(window.event && window.event.currentTarget) {
      window.event.currentTarget.classList.add('active');
    }

    showFloorPins(floorId);
    changeFloorImage(imgId);
  }

  // 画像要素だけを切り替える内部関数
  function changeFloorImage(imgId) {
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
        
        // ピンをクリックした時に、右側のフロアマップ画像も連動して切り替える仕掛け
        marker.on('click', function() {
          changeFloorImage(pin.img);
        });

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
