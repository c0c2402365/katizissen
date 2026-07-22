<style>
  /* 画面幅いっぱいに広げるための設定と、Markdown全体のベース文字サイズ拡大 */
  .markdown-body, .container, body {
    max-width: 98% !important;
    width: 100% !important;
    margin: 0 auto;
    font-size: 21px;
    line-height: 1.7;
  }
  h2 { font-size: 29px !important; margin-top: 30px; }
  h3 { font-size: 25px !important; }
  li { font-size: 21px; margin-bottom: 8px; }
</style>

<div style="display: flex; justify-content: space-between; align-items: center; background: #ffffff; padding: 18px 24px; border-radius: 8px; border: 1px solid #d0d7de; margin-bottom: 25px; flex-wrap: wrap; gap: 15px; box-shadow: 0 2px 4px rgba(0,0,0,0.05);">
  <span style="font-size: 21px; font-weight: bold; color: #2c3e50;">📍 会場マップナビゲーション</span>
  <a href="events.html" style="display: inline-flex; align-items: center; gap: 8px; padding: 12px 24px; background: #27ae60; color: #ffffff; text-decoration: none; border-radius: 6px; font-weight: bold; font-size: 21px; transition: background 0.2s;">
    <span>📅</span> イベントプログラム・出展一覧はこちら ➔
  </a>
</div>

## 🗺️ 会場案内・フロアマップナビゲーション

<p style="font-size: 21px; line-height: 1.6;">見たいエリア・階数を選択すると、アクセスマップとフロアマップ画像が自動で連動して切り替わります。</p>

<div class="manual-nav-box" style="background: #f0f4f8; padding: 22px; border-radius: 8px; margin-bottom: 25px; border: 1px solid #d0d7de;">
  <h4 style="margin: 0 0 16px 0; color: #2c3e50; font-size: 23px; display: flex; align-items: center; gap: 8px;">
    <span>🔍</span> フロア・エリアから直接探す
  </h4>
  <div style="display: flex; gap: 15px; flex-wrap: wrap;">
    <div style="flex: 1; min-width: 250px;">
      <label style="font-size: 19px; color: #444; display: block; margin-bottom: 8px; font-weight: bold;">① フロアを選択</label>
      <select id="floor-select" onchange="onFloorSelectChange(this.value)" style="width: 100%; padding: 12px 16px; border-radius: 6px; border: 1px solid #ced4da; background: #fff; font-size: 21px;">
        <option value="all">全エリア表示（ピン非表示）</option>
        <option value="1f_out">1F 屋外マルシェ・広場</option>
        <option value="1f_in">1F 屋内（ワンストップ・みんなの広場）</option>
        <option value="2f">2F 会議室・おうえん・キッズスペース</option>
        <option value="3f">3F 会議室・アトリウム・議場</option>
        <option value="hall">市民ホール</option>
      </select>
    </div>
    
    <div style="flex: 1; min-width: 250px;">
      <label style="font-size: 19px; color: #444; display: block; margin-bottom: 8px; font-weight: bold;">② エリア・ブースを選択</label>
      <select id="area-select" onchange="onAreaSelectChange(this.value)" style="width: 100%; padding: 12px 16px; border-radius: 6px; border: 1px solid #ced4da; background: #fff; font-size: 21px;" disabled>
        <option value="">フロアを先に選択してください</option>
      </select>
    </div>
  </div>
</div>

<div class="map-controls" style="margin-bottom: 25px;">
  <button class="floor-btn active" data-floor="all" onclick="switchFloorMap('all', 'all_img')">全エリア</button>
  <button class="floor-btn" data-floor="1f_out" onclick="switchFloorMap('1f_out', 'all_img')">1F 屋外</button>
  <button class="floor-btn" data-floor="1f_in" onclick="switchFloorMap('1f_in', 'floor1_img')">1F 屋内</button>
  <button class="floor-btn" data-floor="2f" onclick="switchFloorMap('2f', 'floor2_img')">2F</button>
  <button class="floor-btn" data-floor="3f" onclick="switchFloorMap('3f', 'floor3_img')">3F</button>
  <button class="floor-btn" data-floor="hall" onclick="switchFloorMap('hall', 'all_img')">市民ホール</button>
</div>

<div class="map-flex-container" style="display: flex; flex-wrap: wrap; gap: 30px;">
  
  <div class="map-block" style="flex: 1; min-width: 400px;">
    <h3 style="margin-top: 0; font-size: 25px;">📍 アクセスマップ</h3>
    <div id="event-map" style="width: 100%; height: 600px; border-radius: 8px; border: 1px solid #ccc; z-index: 1;"></div>
  </div>

  <div class="image-block" style="flex: 1; min-width: 400px; text-align: center;">
    <h3 style="margin-top: 0; text-align: left; font-size: 25px;">🏢 庁舎内・会場レイアウト</h3>
    
    <div id="all_img" class="floor-img-content" style="display: block; padding: 80px 30px; background: #f8f9fa; border: 1px dashed #ccc; border-radius: 8px;">
      <p style="color: #555; margin: 0; font-size: 21px; line-height: 1.6;">
        上の選択メニューまたはボタンから<b>「1F屋内」「2F」「3F」</b>を選択すると、<br>
        ここに詳細なフロアマップ（画像）が表示されます。<br><br>
        選択されたエリアのみ地図上にピンが表示されます！
      </p>
    </div>

    <div id="floor1_img" class="floor-img-content" style="display: none; position: relative; max-width: 100%; width: 100%;">
      <img src="1F.jpeg" alt="町田市役所 1階フロアマップ" style="width: 100%; height: auto; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); display: block;">
      <div class="inner-pins"></div>
    </div>
    
    <div id="floor2_img" class="floor-img-content" style="display: none; position: relative; max-width: 100%; width: 100%;">
      <img src="2F_floor1.jpg" alt="町田市役所 2階フロアマップ" style="width: 100%; height: auto; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); display: block;">
      <div class="inner-pins"></div>
    </div>
    
    <div id="floor3_img" class="floor-img-content" style="display: none; position: relative; max-width: 100%; width: 100%;">
      <img src="FloorMAP3F.jpg" alt="町田市役所 3階フロアマップ" style="width: 100%; height: auto; border: 1px solid #ddd; border-radius: 6px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); display: block;">
      <div class="inner-pins"></div>
    </div>
  </div>

</div>

<div id="floor-pin-info" style="margin-top: 25px; padding: 20px 24px; background: #fffcf5; border-left: 6px solid #e67e22; border-radius: 6px; display: none;">
  <strong id="info-title" style="color: #d35400; font-size: 23px; display: block;"></strong>
  <div id="info-desc" style="margin: 10px 0 0 0; font-size: 19px; color: #333; line-height: 1.6;"></div>
</div>


<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>

<script>
  const csvUrl = 'ここに取得したCSVのURLを貼り付けてください';

  const defaultCoords = [35.545199, 139.442838];
  let map;
  let markerGroup;
  let leafletMarkers = {}; 
  let fetchedEvents = {}; 

  const floorImgMap = {
    'all': 'all_img',
    '1f_out': 'all_img',
    '1f_in': 'floor1_img',
    '2f': 'floor2_img',
    '3f': 'floor3_img',
    'hall': 'all_img'
  };

  const pinData = [
    { id: 1, floor: '1f_out', img: 'all_img', name: '1F 屋外マルシェ', coords: [35.54535, 139.44265], desc: '【正面入口前・屋外】新鮮野菜販売、草木染め、各NPOの物販・体験など多数出展！', mapPos: null },
    { id: 2, floor: '1f_out', img: 'all_img', name: '1F 屋外こもれび広場', coords: [35.54540, 139.44285], desc: '【北側・屋外】法政大学焼きそば屋台、各種キッチンカー、リフトカー体験など。', mapPos: null },
    { id: 3, floor: '1f_in',  img: 'floor1_img', name: '1F みんなの広場', coords: [35.54515, 139.44275], desc: '【1階右下エリア】認知症悩みごと相談、古本リサイクル、遺産・相続無料相談会など。', mapPos: { top: '70%', left: '78%' } },
    { id: 4, floor: '1f_in',  img: 'floor1_img', name: '1F ワンストップロビー', coords: [35.54510, 139.44295], desc: '【1階中央（案内所・エレベーター周辺）】謎解きスタンプラリー、ボッチャ体験、防災クイズなど。', mapPos: { top: '48%', left: '48%' } },
    { id: 5, floor: '2f',     img: 'floor2_img', name: '2F 市民協働おうえんルーム', coords: [35.54522, 139.44290], desc: '【2階下側】助産師相談、陽だまりカフェ、スマホ相談、脳疲労ケア体験など。', mapPos: { top: '80%', left: '47%' } },
    { id: 6, floor: '2f',     img: 'floor2_img', name: '2F 北側廊下・キッズスペース・東側廊下', coords: [35.54525, 139.44305], desc: '【2階右上・208-210エリア】ミニまちだ駄菓子屋、おしごとなりきり、クリスマスオーナメント工作、里親制度案内など。', mapPos: { top: '30%', left: '73%' } },
    { id: 7, floor: '2f',     img: 'floor2_img', name: '2F 会議室（2-1 〜 2-4）', coords: [35.54530, 139.44298], desc: '【2階左側・赤色202-204エリア】マッサージ体験、点字、iPhone教室、マイクラ農体験、演劇WS。', mapPos: { top: '50%', left: '30%' } },
    { id: 8, floor: '3f',     img: 'floor3_img', name: '3F 会議室（3-1 〜 3-3）', coords: [35.54505, 139.44270], desc: '【3階上側・302号室周辺】エンディングウェア発表会、ブラックライトアート空間、ひなた村出張科学クラブなど。', mapPos: { top: '16%', left: '67%' } },
    { id: 9, floor: '3f',     img: 'floor3_img', name: '3F アトリウム', coords: [35.54498, 139.44285], desc: '【3階中央大きな広場】革小物WS、リス園写真展示、手話サークル、やさしい日本語クイズ、無料相談。', mapPos: { top: '61%', left: '44%' } },
    { id: 10, floor: '3f',    img: 'floor3_img', name: '3F 議場', coords: [35.54500, 139.44300], desc: '【3階右側・議場】10:15〜 いのちの授業、13:30〜 マチーダ楽団ラテンジャズ（※要予約）。', mapPos: { top: '45%', left: '82%' } },
    { id: 11, floor: 'hall',  img: 'all_img', name: '町田市民ホール', coords: [35.54394, 139.44111], desc: '【市役所から西へ徒歩3分】会議室1〜5にて、ヨガ、ナリワイ大集合、絵本読み聞かせ、盆おどりなど開催！', mapPos: null }
  ];

  document.addEventListener("DOMContentLoaded", function() {
    map = L.map('event-map').setView(defaultCoords, 17);
    
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>',
      detectRetina: true
    }).addTo(map);

    markerGroup = L.layerGroup().addTo(map);
    showFloorPins('all'); // 初期状態はピン非表示
    renderImagePins();
    
    loadCSVData();
  });

  function loadCSVData() {
    if (csvUrl === 'ここに取得したCSVのURLを貼り付けてください' || csvUrl === '') {
      return;
    }
    
    Papa.parse(csvUrl, {
      download: true,
      header: true,
      complete: function(results) {
        const data = results.data;
        data.forEach(row => {
          const area = row['出展エリア'];
          if (area) {
            if (!fetchedEvents[area]) {
              fetchedEvents[area] = [];
            }
            fetchedEvents[area].push({
              name: row['団体名'],
              content: row['出展内容'],
              genre: row['ジャンル']
            });
          }
        });
      }
    });
  }

  function onFloorSelectChange(floorId) {
    const imgId = floorImgMap[floorId] || 'all_img';
    switchFloorMap(floorId, imgId, true);
  }

  function onAreaSelectChange(pinId) {
    if (!pinId) return;
    const pin = pinData.find(p => p.id == pinId);
    if (pin) {
      triggerPinSelection(pin);
    }
  }

  function switchFloorMap(floorId, imgId, isFromDropdown = false) {
    const buttons = document.getElementsByClassName('floor-btn');
    for (let btn of buttons) {
      btn.classList.remove('active');
      if (btn.getAttribute('data-floor') === floorId) {
        btn.classList.add('active');
      }
    }
    if (!isFromDropdown) {
      document.getElementById('floor-select').value = floorId;
    }
    updateAreaSelectDropdown(floorId);
    showFloorPins(floorId);
    changeFloorImage(imgId);
    document.getElementById('floor-pin-info').style.display = 'none';
  }

  function updateAreaSelectDropdown(floorId) {
    const areaSelect = document.getElementById('area-select');
    areaSelect.innerHTML = '';
    if (floorId === 'all') {
      areaSelect.innerHTML = '<option value="">フロアを先に選択してください</option>';
      areaSelect.disabled = true;
    } else {
      areaSelect.disabled = false;
      const defaultOption = document.createElement('option');
      defaultOption.value = "";
      defaultOption.textContent = "エリア・ブースを選択してください";
      areaSelect.appendChild(defaultOption);

      const filtered = pinData.filter(p => p.floor === floorId);
      filtered.forEach(pin => {
        const opt = document.createElement('option');
        opt.value = pin.id;
        opt.textContent = pin.name;
        areaSelect.appendChild(opt);
      });
    }
  }

  // ★「全エリア」ではピンを消し、フロアが選択された時のみ該当ピンを表示する
  function showFloorPins(floorId) {
    markerGroup.clearLayers();
    leafletMarkers = {}; 

    // 「全エリア」選択時はマップ上のピンをゼロにする
    if (floorId === 'all') {
      map.panTo(defaultCoords);
      return;
    }

    pinData.forEach(function(pin) {
      if (pin.floor === floorId) {
        const marker = L.marker(pin.coords);
        marker.bindPopup(`<b style="font-size: 18px;">${pin.name}</b><br><span style="font-size:17px; color:#555; line-height: 1.4; display: inline-block; margin-top: 5px;">${pin.desc}</span>`);
        
        marker.on('click', function() {
          triggerPinSelection(pin);
        });

        markerGroup.addLayer(marker);
        leafletMarkers[pin.id] = marker;
      }
    });

    if (floorId === 'hall') {
      map.panTo([35.54394, 139.44111]);
    } else {
      map.panTo(defaultCoords);
    }
  }

  // ★個別のエリアが選択された時：該当する1つのピンのみを表示・ポップアップ表示する
  function triggerPinSelection(pin) {
    changeFloorImage(pin.img);
    highlightImagePin(pin.id);

    // 単一ピン表示処理
    markerGroup.clearLayers();
    leafletMarkers = {};

    const marker = L.marker(pin.coords);
    marker.bindPopup(`<b style="font-size: 18px;">${pin.name}</b><br><span style="font-size:17px; color:#555; line-height: 1.4; display: inline-block; margin-top: 5px;">${pin.desc}</span>`);
    markerGroup.addLayer(marker);
    leafletMarkers[pin.id] = marker;
    
    marker.openPopup();
    map.panTo(pin.coords);

    // 詳細情報表示
    document.getElementById('info-title').innerText = pin.name;
    let descHtml = `<p style="margin: 8px 0 12px 0; font-size: 18px; color: #555;">${pin.desc}</p>`;
    
    if (fetchedEvents[pin.name] && fetchedEvents[pin.name].length > 0) {
      descHtml += `<div style="margin-top: 18px; border-top: 1px dashed #ccc; padding-top: 15px;">`;
      descHtml += `<strong style="font-size: 20px; color: #2c3e50;">📋 出展・イベント一覧</strong>`;
      descHtml += `<ul style="margin: 10px 0 0 0; padding-left: 24px; font-size: 19px; color: #333;">`;
      
      fetchedEvents[pin.name].forEach(ev => {
        const genreBadge = ev.genre ? `<span style="font-size: 16px; color: #fff; background: #27ae60; padding: 3px 8px; border-radius: 12px; margin-left: 8px; vertical-align: middle;">${ev.genre}</span>` : '';
        descHtml += `<li style="margin-bottom: 12px;">
                       <b style="color:#d35400;">${ev.name || '名称不明'}</b> ${genreBadge}<br>
                       <span style="color: #444; font-size: 18px;">${ev.content || ''}</span>
                     </li>`;
      });
      descHtml += `</ul></div>`;
    }

    document.getElementById('info-desc').innerHTML = descHtml;
    document.getElementById('floor-pin-info').style.display = 'block';
    document.getElementById('area-select').value = pin.id;
  }

  function changeFloorImage(imgId) {
    const imgContents = document.getElementsByClassName('floor-img-content');
    for (let img of imgContents) { img.style.display = 'none'; }
    const targetImg = document.getElementById(imgId);
    if (targetImg) {
      targetImg.style.display = (imgId === 'all_img') ? 'block' : 'inline-block';
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
            triggerPinSelection(pin);
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
  .leaflet-marker-icon,
  .leaflet-marker-shadow {
    background: transparent !important;
    border: none !important;
    box-shadow: none !important;
    padding: 0 !important;
  }

  .floor-btn {
    padding: 12px 22px;
    font-size: 20px;
    font-weight: 500;
    cursor: pointer;
    background: #f7f9fa;
    border: 1px solid #ced4da;
    border-radius: 20px;
    margin: 6px;
    transition: all 0.2s;
  }
  .floor-btn.active {
    background: #e67e22;
    color: white;
    border-color: #e67e22;
    font-weight: bold;
  }
  .floor-btn:hover {
    background: #e2e8f0;
  }

  .floor-image-pin {
    position: absolute;
    width: 24px;
    height: 24px;
    background-color: rgba(255, 59, 48, 0.85);
    border: 2px solid rgba(255, 255, 255, 0.9);
    border-radius: 50%;
    cursor: pointer;
    transform: translate(-50%, -50%);
    box-shadow: 0 2px 5px rgba(0,0,0,0.4);
    transition: transform 0.2s, background-color 0.2s;
    z-index: 10;
  }
  .floor-image-pin:hover {
    transform: translate(-50%, -50%) scale(1.3);
    background-color: rgba(230, 126, 34, 0.9);
  }
  .floor-image-pin.selected {
    background-color: rgba(230, 126, 34, 1);
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
