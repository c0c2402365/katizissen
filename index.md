## 🗺️ 会場案内・フロアマップナビゲーション

<p>見たいエリア・階数のボタンを押すと、アクセスマップのピン位置と、右側の詳細なフロアマップ画像が同時に切り替わります。また、地図上のピンやフロアマップ上のピンをクリックすると各スペースの出展概要が表示されます。</p>

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
    
    <div style="text-align: center; margin-top: 10px;">
      <a href="https://www.google.com/maps/dir/?api=1&destination=35.545199,139.442838" target="_blank" class="route-btn">
        🚶 現在地から市役所へのルート案内を開く
      </a>
    </div>
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

    <div id="floor1_img" class="floor-img-content" style="display: none; position: relative; display: inline-block; max-width: 450px; width: 100%;">
      <img src="1F.jpeg" alt="町田市役所 1階フロアマップ" style="width: 100%; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
      <div class="inner-pins"></div>
    </div>
    
    <div id="floor2_img" class="floor-img-content" style="display: none; position: relative; display: inline-block; max-width: 450px; width: 100%;">
      <img src="2F_floor1.jpg" alt="町田市役所 2階フロアマップ" style="width: 100%; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
      <div class="inner-pins"></div>
    </div>
    
    <div id="floor3_img" class="floor-img-content" style="display: none; position: relative; display: inline-block; max-width: 450px; width: 100%;">
      <img src="FloorMAP3F.jpg" alt="町田市役所 3階フロアマップ" style="width: 100%; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
      <div class="inner-pins"></div>
    </div>
  </div>

</div>

<div id="floor-pin-info" style="margin-top: 15px; padding: 10px 15px; background: #fffcf5; border-left: 4px solid #e67e22; border-radius: 4px; display: none;">
  <strong id="info-title" style="color: #d35400;"></strong>
  <p id="info-desc" style="margin: 5px 0 0 0; font-size: 13px; color: #333;"></p>
</div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const defaultCoords = [35.545199, 139.442838];
  let map;
  let markerGroup;
  let routeGroup; // ルート案内線用のグループを追加
  let leafletMarkers = {}; 

  const pinData = [
    { id: 1, floor: '1f_out', img: 'all_img', name: '1F 屋外マルシェ', coords: [35.54535, 139.44265], desc: '【正面入口前・屋外】新鮮野菜販売、草木染め、各NPOの物販・体験など多数出展！', mapPos: null },
    { id: 2, floor: '1f_out', img: 'all_img', name: '1F 屋外ここもれび広場', coords: [35.54540, 139.44285], desc: '【北側・屋外】法政大学焼きそば屋台、各種キッチンカー、リフトカー体験など。', mapPos: null },
    { id: 3, floor: '1f_in',  img: 'floor1_img', name: '1F みんなの広場', coords: [35.54515, 139.44275], desc: '【1階右下エリア】認知症悩みごと相談、古本リサイクル、遺産・相続無料相談会など。', mapPos: { top: '75%', left: '70%' } },
    { id: 4, floor: '1f_in',  img: 'floor1_img', name: '1F ワンストップロビー', coords: [35.54510, 139.44295], desc: '【1階中央（案内所・エレベーター周辺）】謎解きスタンプラリー、ボッチャ体験、防災クイズなど。', mapPos: { top: '45%', left: '50%' } },
    { id: 5, floor: '2f',     img: 'floor2_img', name: '2F 市民協働おうえんルーム', coords: [35.54522, 139.44290], desc: '【2階下側（201横）】助産師相談、陽だまりカフェ、スマホ相談、脳疲労ケア体験など。', mapPos: { top: '82%', left: '45%' } },
    { id: 6, floor: '2f',     img: 'floor2_img', name: '2F 北側廊下・キッズスペース・東側廊下', coords: [35.54525, 139.44305], desc: '【2階中央〜右側】ミニまちだ駄菓子屋、おしごとなりきり、クリスマスオーナメント工作、里親制度案内など。', mapPos: { top: '33%', left: '79%' } },
    { id: 7, floor: '2f',     img: 'floor2_img', name: '2F 会議室（2-1 〜 2-4）', coords: [35.54530, 139.44298], desc: '【2階左側・赤色202-204エリア】マッサージ体験、点字、iPhone教室、マイクラ農体験、演劇WS。', mapPos: { top: '48%', left: '30%' } },
    { id: 8, floor: '3f',     img: 'floor3_img', name: '3F 会議室（3-1 〜 3-3）', coords: [35.54505, 139.44270], desc: '【3階上側・302近辺】エンディングウェア発表会、ブラックライトアート空間、ひなた村出張科学クラブなど。', mapPos: { top: '35%', left: '25%' } },
    { id: 9, floor: '3f',     img: 'floor3_img', name: '3F アトリウム', coords: [35.54498, 139.44285], desc: '【3階中央大きな広場】革小物WS、リス園写真展示、手話サークル、やさしい日本語クイズ、無料相談。', mapPos: { top: '48%', left: '50%' } },
    { id: 10, floor: '3f',    img: 'floor3_img', name: '3F 議場', coords: [35.54500, 139.44300], desc: '【3階右側・議場】10:15〜 いのちの授業、13:30〜 マチーダ楽団ラテンジャズ（※要予約）。', mapPos: { top: '65%', left: '82%' } },
    { id: 11, floor: 'hall',  img: 'all_img', name: '町田市民ホール（サテライト会場）', coords: [35.54394, 139.44111], desc: '【市役所から西へ徒歩3分】会議室1〜5にて、ヨガ、ナリワイ大集合、絵本読み聞かせ、盆おどりなど開催！', mapPos: null }
  ];

  // ▼ 駅から会場までの案内ルート座標を追加 ▼
  const walkingRoutes = [
    // 小田急町田駅 → 市役所のルート
    [[35.54400, 139.44430], [35.54440, 139.44385], [35.54500, 139.44315], [35.54519, 139.44283]],
    // JR町田駅 → 小田急合流点のルート
    [[35.54260, 139.44560], [35.54330, 139.44490], [35.54400, 139.44430]],
    // 市役所 → 市民ホールのルート
    [[35.54519, 139.44283], [35.54480, 139.44250], [35.54420, 139.44130], [35.54394, 139.44111]]
  ];

  document.addEventListener("DOMContentLoaded", function() {
    map = L.map('event-map').setView(defaultCoords, 16);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map);

    markerGroup = L.layerGroup().addTo(map);
    routeGroup = L.layerGroup().addTo(map); // ルート線用グループ

    // 駅のマーカーを追加
    drawStationMarkers();

    showFloorPins('all');
    renderImagePins();
  });

  // ▼ 駅のピンとルート線を地図に描画する関数 ▼
  function drawStationMarkers() {
    // 小田急線町田駅
    L.marker([35.54400, 139.44430], { opacity: 0.8 })
      .bindPopup("<b>🚉 小田急線 町田駅（西口）</b><br>ここから徒歩約8分")
      .addTo(routeGroup);
    
    // JR横浜線町田駅
    L.marker([35.54260, 139.44560], { opacity: 0.8 })
      .bindPopup("<b>🚉 JR横浜線 町田駅（北口）</b><br>ここから徒歩約11分")
      .addTo(routeGroup);

    // 徒歩ルートを点線で描画
    walkingRoutes.forEach(route => {
      L.polyline(route, { 
        color: '#4285f4', 
        weight: 5, 
        opacity: 0.7, 
        dashArray: '8, 8' 
      }).addTo(routeGroup);
    });
  }

  function switchFloorMap(floorId, imgId) {
    const buttons = document.getElementsByClassName('floor-btn');
    for (let btn of buttons) { btn.classList.remove('active'); }
    
    if(window.event && window.event.currentTarget) {
      window.event.currentTarget.classList.add('active');
    }

    showFloorPins(floorId);
    changeFloorImage(imgId);
  }

  function changeFloorImage(imgId) {
    const imgContents = document.getElementsByClassName('floor-img-content');
    for (let img of imgContents) { img.style.display = 'none'; }
    
    const targetImg = document.getElementById(imgId);
    if (targetImg) {
      targetImg.style.display = (imgId === 'all_img') ? 'block' : 'inline-block';
    }
  }

  function showFloorPins(floorId) {
    markerGroup.clearLayers();
    leafletMarkers = {}; 

    pinData.forEach(function(pin) {
      if (floorId === 'all' || pin.floor === floorId) {
        const marker = L.marker(pin.coords);
        
        // ポップアップの中にGoogle Mapsへのリンクを追加
        const popupContent = `
          <b>${pin.name}</b><br>
          <span style="font-size:12px; color:#555;">${pin.desc}</span><br>
          <a href="https://www.google.com/maps/dir/?api=1&destination=${pin.coords[0]},${pin.coords[1]}" target="_blank" style="display:inline-block; margin-top:5px; font-size:11px; color:#0066cc;">
            ↗ Google Mapsでここへのルートを見る
          </a>
        `;
        marker.bindPopup(popupContent);
        
        marker.on('click', function() {
          changeFloorImage(pin.img);
          highlightImagePin(pin.id);
        });

        markerGroup.addLayer(marker);
        leafletMarkers[pin.id] = marker;
      }
    });

    if (floorId === 'hall') {
      map.setView([35.54394, 139.44111], 17); // 市民ホールにフォーカス
    } else {
      map.setView(defaultCoords, 16); // 全体が見えるようにズームを16に少し引く
    }
  }

  function renderImagePins() {
    document.querySelectorAll('.inner-pins').forEach(el => el.innerHTML = '');

    pinData.forEach(function(pin) {
      if (pin.mapPos) {
        const container = document.querySelector(`#${pin.img} .inner-pins`);
        if (container) {
          const pinEl = document.createElement('div');
          pinEl.className = `floor-image-pin pin-id-${pin.id}`;
          pinEl.style.top = pin.mapPos.top;
          pinEl.style.left = pin.mapPos.left;
          pinEl.title = pin.name;

          pinEl.addEventListener('click', function() {
            document.getElementById('info-title').innerText = pin.name;
            document.getElementById('info-desc').innerText = pin.desc;
            document.getElementById('floor-pin-info').style.display = 'block';

            if (leafletMarkers[pin.id]) {
              leafletMarkers[pin.id].openPopup();
              map.panTo(pin.coords);
            }

            highlightImagePin(pin.id);
          });

          container.appendChild(pinEl);
        }
      }
    });
  }

  function highlightImagePin(activeId) {
    document.querySelectorAll('.floor-image-pin').forEach(el => {
      el.classList.remove('selected');
    });
    const activePin = document.querySelector(`.floor-image-pin.pin-id-${activeId}`);
    if (activePin) {
      activePin.classList.add('selected');
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

  /* ルート案内ボタンの装飾 */
  .route-btn {
    display: inline-block;
    padding: 10px 18px;
    background-color: #4285f4;
    color: white;
    text-decoration: none;
    border-radius: 24px;
    font-weight: bold;
    font-size: 14px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    transition: background-color 0.2s;
  }
  .route-btn:hover {
    background-color: #3367d6;
    color: white;
  }

  .floor-image-pin {
    position: absolute;
    width: 18px;
    height: 18px;
    background-color: #ff3b30;
    border: 2px solid white;
    border-radius: 50%;
    cursor: pointer;
    transform: translate(-50%, -50%);
    box-shadow: 0 2px 5px rgba(0,0,0,0.4);
    transition: transform 0.2s, background-color 0.2s;
    z-index: 10;
  }
  .floor-image-pin:hover {
    transform: translate(-50%, -50%) scale(1.3);
    background-color: #e67e22;
  }
  .floor-image-pin.selected {
    background-color: #e67e22;
    transform: translate(-50%, -50%) scale(1.4);
    box-shadow: 0 0 0 4px rgba(230, 126, 34, 0.4);
  }
</style>

---
## 🗺️ アクセス

### メイン会場：町田市役所本庁舎
* 〒194-8520 東京都町田市森野2-2-22
* 小田急線町田駅「西口」から徒歩約8分
* JR横浜線町田駅「北口」から徒歩約11分
> ※イベント当日は混雑が予想されます。公共交通機関でのご来場にご協力をお願いいたします。
