<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manisa Takas Ağı - PRO Sürüm</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
    <style>
        /* Genel Stil Ayarları */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f4f6f8; /* Açık Gri Arka Plan */
            margin: 0;
            color: #1f2937;
        }
        /* Başlık Çubuğu */
        .header {
            background: #1e3a8a; /* Koyu Lacivert */
            color: white;
            padding: 25px 20px;
            font-size: 32px;
            font-weight: 800;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            text-align: center;
        }
        /* Ana Buton Stilleri */
        .main-btn {
            padding: 12px 25px;
            font-size: 18px;
            border: none;
            border-radius: 8px;
            color: white;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: all 0.2s;
            font-weight: 600;
        }
        .main-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }
        /* Modal Stili */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 9999;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow-y: auto;
        }
        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            max-width: 95%;
            width: 550px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }
        /* İlan Kartı Stili */
        .card {
            background: white;
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s, box-shadow 0.3s;
            cursor: pointer;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
        }
        .card-img {
            width: 100%;
            height: 200px;
            object-fit: cover;
        }
        /* Gizli İletişim Stili */
        .gizli-iletisim {
            background: #fef3c7; /* Yumuşak Sarı Arkaplan */
            color: #b45309; /* Koyu Turuncu Yazı */
            padding: 15px;
            border-radius: 8px;
            font-weight: 600;
            text-align: center;
            border: 1px solid #fcd34d;
        }
        /* Form Alanları */
        .form-input {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            border-radius: 8px;
            border: 1px solid #d1d5db;
            margin: 8px 0;
            transition: border-color 0.2s, box-shadow 0.2s;
            background: #f9fafb;
        }
        .form-input:focus {
            border-color: #1e3a8a;
            box-shadow: 0 0 0 3px rgba(30, 58, 138, 0.2);
            outline: none;
        }
        .tab-btn {
            flex:1;
            padding:15px 0;
            font-size:19px;
            font-weight:600;
            cursor:pointer;
            transition:color 0.3s, border-bottom 0.3s;
            background:white;
        }
        .tab-active {
            color:#1e3a8a;
            border-bottom:3px solid #1e3a8a;
        }
        .tab-inactive {
            color:#6b7280;
            border-bottom:3px solid transparent;
        }
    </style>
</head>
<body>
    <div class="header">
        <i class="fas fa-handshake"></i> Manisa Takas Ağı
        <span style="font-size:14px;display:block;font-weight:300;">Profesyonel İlan Platformu</span>
    </div>

    <div style="padding:20px;text-align:center;background:white;box-shadow:0 2px 10px rgba(0,0,0,0.05);">
        <button class="main-btn" style="background:#b91c1c;" onclick="adminGiris()">
            <i class="fas fa-user-shield"></i> Admin
        </button>
        <button class="main-btn" style="background:#374151;" onclick="profilDuzenle()">
            <i class="fas fa-user-circle"></i> Profil: ${JSON.parse(localStorage.getItem('takas_profil') || '{"ad":"Misafir"}').ad}
        </button>
        <button class="main-btn" style="background:#059669;font-size:24px;padding:15px 35px;" onclick="ilanEkleAc()">
            <i class="fas fa-plus"></i> İlan Ekle
        </button>
    </div>

    <div style="padding:20px;background:white;margin:20px;border-radius:12px;box-shadow:0 4px 20px rgba(0,0,0,0.1);">
        
        <input type="text" id="ara" onkeyup="goster()" placeholder="İlanlarda Ara (Başlık, Açıklama)..." class="form-input" style="margin-bottom:15px;">
        
        <div style="display:flex;gap:15px;margin-bottom:20px;">
            <select id="kategoriFiltre" class="form-input" onchange="goster()">
                <option value="">Kategori Filtresi</option>
                <option value="Elektronik">Elektronik</option>
                <option value="Araç">Araç & Yedek Parça</option>
                <option value="Emlak">Emlak</option>
                <option value="Hizmet">Hizmet</option>
                <option value="Giyim">Giyim</option>
                <option value="Diğer">Diğer</option>
            </select>
            <select id="ilceFiltre" class="form-input" onchange="goster()">
                <option value="">İlçe Filtresi (Manisa)</option>
                <option value="Merkez">Şehzadeler/Yunusemre</option>
                <option value="Akhisar">Akhisar</option>
                <option value="Salihli">Salihli</option>
                <option value="Turgutlu">Turgutlu</option>
                <option value="Soma">Soma</option>
                <option value="Alaşehir">Alaşehir</option>
                <option value="Kula">Kula</option>
                <option value="Demirci">Demirci</option>
                <option value="Manisa Dışı">MANİSA DIŞI</option>
            </select>
            </div>

        <div style="display:flex;border-bottom:1px solid #e5e7eb;">
            <button id="hepsi-btn" class="tab-btn" onclick="sekme='hepsi';goster()">Tüm İlanlar</button>
            <button id="benim-btn" class="tab-btn" onclick="sekme='benim';goster()">Benim İlanlarım</button>
        </div>
    </div>

    <div style="padding:20px;display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:30px" id="ilanlar"></div>

    <div class="modal" id="modalEkle">
        <div class="modal-content">
            <h2 style="font-size:30px;font-weight:800;text-align:center;margin-bottom:20px;color:#1e3a8a;">Yeni İlan Yayınla</h2>
            
            <div style="padding:15px;background:#eef2ff;border-radius:10px;margin-bottom:20px;font-size:14px;color:#3730a3;">
                <p style="font-weight:bold;"><i class="fas fa-info-circle"></i> Komisyon Bilgisi:</p>
                <p>İlan onayından sonra iletişim bilgilerinizin açılması için **20 TL** komisyon ödemesi gereklidir. Ödeme bilgileri: **IBAN: TR16 0006 7010 0000 0030 8377 59 (Sibel Kaya)**</p>
            </div>

            <input type="file" id="foto" accept="image/*" onchange="fotoGoster(this)" style="display:block;margin-bottom:10px;width:100%;"><br>
            <img id="onizleme" style="max-height:180px;border-radius:10px;display:none;margin:0 auto 15px auto;border:1px solid #ccc;"><br>
            
            <select id="kategori" required class="form-input">
                <option value="">* Kategori Seçin</option>
                <option value="Elektronik">Elektronik</option>
                <option value="Araç">Araç & Yedek Parça</option>
                <option value="Emlak">Emlak</option>
                <option value="Hizmet">Hizmet</option>
                <option value="Giyim">Giyim</option>
                <option value="Diğer">Diğer</option>
            </select>
            <select id="ilce" required class="form-input">
                <option value="">* Manisa İlçesi Seçin</option>
                <option value="Merkez">Şehzadeler/Yunusemre</option>
                <option value="Akhisar">Akhisar</option>
                <option value="Salihli">Salihli</option>
                <option value="Turgutlu">Turgutlu</option>
                <option value="Soma">Soma</option>
                <option value="Alaşehir">Alaşehir</option>
                <option value="Kula">Kula</option>
                <option value="Demirci">Demirci</option>
                <option value="Manisa Dışı">MANİSA DIŞI (Diğer Şehirler)</option>
            </select>
            <input type="text" id="baslik" placeholder="* Başlık (Zorunlu)" class="form-input"><br>
            <input type="number" id="fiyat" placeholder="* Değer (TL)" class="form-input"><br>
            <textarea id="ver" placeholder="* Ne veriyorsun? (Detaylı Açıklama)" class="form-input" style="height:90px;"></textarea><br>
            <textarea id="istek" placeholder="* Ne istiyorsun? (Neye Takas Edilir?)" class="form-input" style="height:90px;"></textarea><br>
            <input type="text" id="iletisim" placeholder="* Telefon/Instagram (Gizli Kalır)" class="form-input"><br>
            
            <div style="display:flex;gap:15px;margin-top:20px;">
                <button class="main-btn" style="background:#1e3a8a;flex:1;z-index:10000;" onclick="ilanEkle()">YAYINLA</button>
                <button class="main-btn" style="background:#6b7280;flex:1;" onclick="modalEkle.style.display='none'">İptal</button>
            </div>
        </div>
    </div>

    <div class="modal" id="modalDetay">
        <div class="modal-content" style="max-width:700px;">
            <div id="detayIcerik"></div>
            <button class="main-btn" style="background:#6b7280;margin-top:20px;width:100%;" onclick="modalDetay.style.display='none'">Kapat</button>
        </div>
    </div>

    <script>
        let ilanlar = JSON.parse(localStorage.getItem('takas_full') || '[]');
        let profil = JSON.parse(localStorage.getItem('takas_profil') || '{"ad":"Misafir","tel":""}');
        let sekme = 'hepsi';
        const modalEkle = document.getElementById('modalEkle');
        const modalDetay = document.getElementById('modalDetay');

        const IBAN_INFO = "Yapı Kredi - Sibel Kaya - IBAN: TR16 0006 7010 0000 0030 8377 59";

        function notify(m, isError = false) { 
            const n = document.createElement('div'); 
            const bgColor = isError ? '#ef4444' : '#10b981';
            n.style.cssText = `position:fixed;bottom:20px;right:20px;padding:15px 30px;background:${bgColor};color:white;border-radius:50px;z-index:9999;font-weight:bold;box-shadow: 0 4px 10px rgba(0,0,0,0.3);`; 
            n.textContent = m; 
            document.body.appendChild(n); 
            setTimeout(() => n.remove(), 4000); 
        }
        
        function fotoGoster(i) {
            const onizleme = document.getElementById('onizleme');
            if (i.files[0]) {
                const r = new FileReader();
                r.onload = e => {
                    onizleme.src = e.target.result;
                    onizleme.style.display = 'block';
                };
                r.readAsDataURL(i.files[0]);
            } else {
                onizleme.style.display = 'none';
            }
        }

        function resizeImage(base64Str, maxWidth = 800, maxHeight = 600, quality = 0.7) {
            return new Promise((resolve) => {
                const img = new Image();
                img.onload = () => {
                    let width = img.width;
                    let height = img.height;

                    if (width > height) {
                        if (width > maxWidth) {
                            height *= maxWidth / width;
                            width = maxWidth;
                        }
                    } else {
                        if (height > maxHeight) {
                            width *= maxHeight / height;
                            height = maxHeight;
                        }
                    }

                    const canvas = document.createElement('canvas');
                    canvas.width = width;
                    canvas.height = height;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0, width, height);
                    
                    resolve(canvas.toDataURL('image/jpeg', quality));
                };
                img.onerror = () => {
                    resolve(base64Str);
                };
                img.src = base64Str;
            });
        }

        function ilanEkle() {
            const kategori = document.getElementById('kategori').value;
            const ilce = document.getElementById('ilce').value;
            const baslik = document.getElementById('baslik').value.trim();
            const fiyat = document.getElementById('fiyat').value.trim();
            const ver = document.getElementById('ver').value.trim();
            const istek = document.getElementById('istek').value.trim();
            const iletisim = document.getElementById('iletisim').value.trim();
            const fotoInput = document.getElementById('foto');

            if (!kategori || !ilce || !baslik || !fiyat || !ver || !istek || !iletisim) {
                notify('Lütfen yıldızlı (*) tüm zorunlu alanları doldurun!', true);
                return;
            }
            if (profil.ad === 'Misafir') {
                notify('İlan yayınlamadan önce lütfen profilinizi düzenleyin!', true);
                return;
            }

            let fotoBase64 = document.getElementById('onizleme').src;
            let finalFotoPromise;

            if (fotoInput.files.length > 0) {
                finalFotoPromise = resizeImage(fotoBase64);
            } else {
                finalFotoPromise = Promise.resolve('https://via.placeholder.com/600x300?text=FOTO+YOK');
            }

            finalFotoPromise.then(sıkıştırılmışFoto => {
                const yeniIlan = {
                    id: Date.now(),
                    foto: sıkıştırılmışFoto, 
                    baslik: baslik, 
                    fiyat: fiyat, 
                    ver: ver, 
                    istek: istek, 
                    iletisim: iletisim, 
                    sahip: profil.ad, 
                    kategori: kategori,
                    ilce: ilce,
                    onaylandi: false, 
                    paraAlindi: false 
                };
                
                try {
                    ilanlar.push(yeniIlan);
                    localStorage.setItem('takas_full', JSON.stringify(ilanlar));
                    notify('İlan gönderildi! Admin onayı bekleniyor.');
                    modalEkle.style.display = 'none';
                    
                    // Formu temizle
                    document.getElementById('baslik').value = '';
                    document.getElementById('fiyat').value = '';
                    document.getElementById('ver').value = '';
                    document.getElementById('istek').value = '';
                    document.getElementById('iletisim').value = '';
                    document.getElementById('onizleme').style.display = 'none';
                    document.getElementById('foto').value = '';
                    document.getElementById('kategori').value = '';
                    document.getElementById('ilce').value = '';

                    goster();

                } catch (e) {
                    notify('Kayıt başarısız! Tarayıcı depolama alanı dolu olabilir. Lütfen eski ilanları temizleyin.', true);
                    ilanlar.pop(); 
                }
            }).catch(e => {
                notify('Fotoğraf işlenirken ciddi bir hata oluştu: ' + e.message, true);
            });
        }

        function goster() {
            document.getElementById('hepsi-btn').className = 'tab-btn ' + (sekme === 'hepsi' ? 'tab-active' : 'tab-inactive');
            document.getElementById('benim-btn').className = 'tab-btn ' + (sekme === 'benim' ? 'tab-active' : 'tab-inactive');

            const ara = document.getElementById('ara').value.toLowerCase();
            const kategoriFiltre = document.getElementById('kategoriFiltre').value;
            const ilceFiltre = document.getElementById('ilceFiltre').value;

            const div = document.getElementById('ilanlar');
            div.innerHTML = '';

            let list = ilanlar.filter(x => {
                if (!x.onaylandi) return false;
                if (sekme === 'benim' && x.sahip !== profil.ad) return false;
                if (kategoriFiltre && x.kategori !== kategoriFiltre) return false;
                if (ilceFiltre && x.ilce !== ilceFiltre) return false;
                
                if (ara && 
                   !x.baslik.toLowerCase().includes(ara) && 
                   !x.ver.toLowerCase().includes(ara) && 
                   !x.istek.toLowerCase().includes(ara)) return false;
                
                return true;
            });

            if (list.length === 0) {
                div.innerHTML = '<p style="grid-column:1/-1;text-align:center;font-size:30px;color:#6b7280;padding:50px"><i class="fas fa-search"></i> Filtrelere uygun ilan bulunamadı.</p>';
                return;
            }

            list.forEach((x, i) => {
                const kart = document.createElement('div');
                kart.className = 'card';
                kart.onclick = () => detayGoster(x.id); 

                kart.innerHTML = `
                    <img src="${x.foto}" class="card-img">
                    <div style="padding:20px">
                        <div style="font-size:14px;color:${x.paraAlindi ? '#059669' : '#f97316'};margin-bottom:8px;font-weight:bold;display:flex;justify-content:space-between;">
                            <span><i class="fas fa-map-marker-alt"></i> ${x.ilce} / ${x.kategori}</span>
                            <span>${x.paraAlindi ? '<i class="fas fa-unlock"></i> İletişim Açık' : '<i class="fas fa-lock"></i> Komisyon Bekleniyor'}</span>
                        </div>
                        <h3 style="font-size:22px;font-weight:800;margin-bottom:5px;">${x.baslik}</h3>
                        <p style="font-size:16px;color:#4b5563;max-height:40px;overflow:hidden;text-overflow:ellipsis;margin-bottom:15px;">**Verdiği:** ${x.ver}</p>
                        
                        <div style="display:flex;justify-content:space-between;align-items:center;margin-top:10px;border-top:1px solid #f3f4f6;padding-top:10px;">
                            <p style="font-size:28px;color:#10b981;font-weight:bolder;">${x.fiyat} ₺</p>
                            <p style="color:#6b7280;font-size:14px;"><i class="fas fa-user"></i> ${x.sahip}</p>
                        </div>
                    </div>
                `;
                div.appendChild(kart);
            });
        }
        
        function detayGoster(id) {
            const ilan = ilanlar.find(i => i.id === id && i.onaylandi);
            if (!ilan) return;

            const iletisimGoster = ilan.paraAlindi 
                ? `<div style="font-size:20px;color:#1e40af;padding:15px;background:#e0f2fe;border-radius:10px;word-break:break-all;text-align:center;"><i class="fas fa-phone-alt"></i> **${ilan.iletisim}**</div>`
                : `<div class="gizli-iletisim"><i class="fas fa-money-check-alt"></i> İletişim Bilgisi, **20 TL** Komisyon Ödemesinden Sonra Açılacaktır.</div>`;

            document.getElementById('detayIcerik').innerHTML = `
                <img src="${ilan.foto}" style="width:100%;max-height:450px;object-fit:cover;border-radius:12px;margin-bottom:20px;">
                <h2 style="font-size:32px;font-weight:800;color:#1f2937;margin-bottom:5px;">${ilan.baslik}</h2>
                <div style="font-size:16px;color:#6b7280;margin-bottom:20px;border-bottom:1px solid #e5e7eb;padding-bottom:10px;">
                    <span style="margin-right:15px;"><i class="fas fa-tag"></i> **${ilan.kategori}**</span>
                    <span><i class="fas fa-map-marker-alt"></i> **${ilan.ilce}**</span>
                </div>
                
                <h3 style="font-size:20px;font-weight:bold;color:#1f2937;">**Verdiği (Detaylı Açıklama)**</h3>
                <p style="font-size:17px;color:#4b5563;margin-bottom:15px;background:#f9fafb;padding:10px;border-radius:6px;">${ilan.ver}</p>
                
                <h3 style="font-size:20px;font-weight:bold;color:#1f2937;">**İstediği (Takas/Nakiyye)**</h3>
                <p style="font-size:17px;color:#4b5563;margin-bottom:25px;background:#f9fafb;padding:10px;border-radius:6px;">${ilan.istek}</p>

                <div style="text-align:center;padding:20px;background:#ecfdf5;border-radius:15px;margin-bottom:20px;">
                    <p style="font-size:55px;color:#047857;font-weight:extrabolder;">${ilan.fiyat} ₺</p>
                    <p style="font-size:18px;color:#065f46;margin-top:-5px;">(İlanın Tahmini Değeri)</p>
                </div>

                <h3 style="font-size:20px;font-weight:bold;color:#1f2937;margin-bottom:10px;">**İletişim Bilgisi**</h3>
                ${iletisimGoster}
                <p style="text-align:right;color:#6b7280;font-size:15px;margin-top:15px;">İlan Sahibi: ${ilan.sahip}</p>
            `;

            modalDetay.style.display = 'flex';
        }

        function profilDuzenle() {
            const ad = prompt('Ad Soyad (Bu adla ilan yayınlayacaksınız):', profil.ad);
            const tel = prompt('Telefon/Instagram (İlanlarınıza hızlı ulaşım için):', profil.tel);
            if (ad) {
                profil = {ad: ad.trim(), tel: tel ? tel.trim() : ""};
                localStorage.setItem('takas_profil', JSON.stringify(profil));
                notify('Profil kaydedildi');
                document.querySelector('.main-btn:nth-child(2)').innerHTML = `<i class="fas fa-user-circle"></i> Profil: ${profil.ad}`;
                goster();
            }
        }

        function adminGiris() {
            const sifre = prompt('Admin şifresi:');
            if (sifre === 'admin2024') {
                let mesaj = "YÖNETİCİ PANELİ - İlan Onaylama ve Komisyon Takibi\n\n";
                mesaj += "KOMİSYON IBAN BİLGİSİ (20 TL Yayınlama Ücreti):\n";
                mesaj += IBAN_INFO + "\n\n";
                
                const bekleyenOnay = ilanlar.filter(x => !x.onaylandi);
                const onayliKomisyonBekleyen = ilanlar.filter(x => x.onaylandi && !x.paraAlindi);
                
                mesaj += "1. YENİ İLANLAR (ONAY BEKLEYEN):\n";
                if (bekleyenOnay.length === 0) mesaj += "Yok\n";
                else {
                    bekleyenOnay.forEach((x, i) => {
                        mesaj += `--- ${i+1}. YENİ İLAN ---\n`;
                        mesaj += `[${x.kategori} / ${x.ilce}] Başlık: ${x.baslik}\n`;
                        mesaj += `Sahip: ${x.sahip}\n`;
                        mesaj += `Onaylamak için ${i+1} yazın.\n`;
                    });
                }

                mesaj += "\n2. ONAYLI İLANLAR (KOMİSYON BEKLEYEN):\n";
                if (onayliKomisyonBekleyen.length === 0) mesaj += "Yok\n";
                else {
                    onayliKomisyonBekleyen.forEach((x, i) => {
                        mesaj += `--- ${i+1}. KOMİSYON İLANI ---\n`;
                        mesaj += `[${x.kategori} / ${x.ilce}] Başlık: ${x.baslik}\n`;
                        mesaj += `Sahip: ${x.sahip}\n`;
                        mesaj += `Komisyonu ON-AY-LA-MAK için A${i+1} yazın.\n`;
                    });
                }
                
                const cevap = prompt(mesaj + "\n\nLütfen işlem yapmak istediğiniz ilan numarasını (Örn: 1 veya A1) giriniz:");
                
                if (!cevap) {
                    notify('Admin paneli kapatıldı.');
                    return;
                }

                let islemTamamlandi = false;

                // İşlem: Komisyon Onayı
                if (cevap.toUpperCase().startsWith('A') && !isNaN(cevap.substring(1))) {
                    const index = parseInt(cevap.substring(1)) - 1;
                    if (onayliKomisyonBekleyen[index]) {
                        const ilanID = onayliKomisyonBekleyen[index].id;
                        const orijinalIndex = ilanlar.findIndex(ilan => ilan.id === ilanID);

                        if (orijinalIndex > -1) {
                            ilanlar[orijinalIndex].paraAlindi = true; 
                            localStorage.setItem('takas_full', JSON.stringify(ilanlar));
                            notify(`İlan (${ilanlar[orijinalIndex].baslik}) için KOMİSYON ONAYI yapıldı. İletişim bilgisi açıldı.`);
                            islemTamamlandi = true;
                        }
                    } else {
                        notify('Geçersiz komisyon ilan numarası.');
                    }
                } 
                // İşlem: Yeni İlan Onayı
                else if (!isNaN(cevap)) {
                    const index = parseInt(cevap) - 1;
                    if (bekleyenOnay[index]) {
                        const ilanID = bekleyenOnay[index].id;
                        const orijinalIndex = ilanlar.findIndex(ilan => ilan.id === ilanID);

                        if (orijinalIndex > -1) {
                            ilanlar[orijinalIndex].onaylandi = true;
                            localStorage.setItem('takas_full', JSON.stringify(ilanlar));
                            notify(`Yeni İlan (${ilanlar[orijinalIndex].baslik}) genel olarak onaylandı. Komisyon ödemesi bekleniyor.`);
                            islemTamamlandi = true;
                        }
                    } else {
                        notify('Geçersiz yeni ilan numarası.');
                    }
                } 
                
                if (islemTamamlandi) {
                    goster();
                } else if (cevap !== null) {
                    notify('Geçersiz işlem kodu.');
                }

            } else if (sifre !== null) {
                notify('Hatalı şifre.');
            }
        }

        function ilanEkleAc() {
            modalEkle.style.display = 'flex';
        }

        goster();
    </script>
</body>
</html>
