
1. MANIFEST.JSON : 
    
    Manifestte izinleri olmayan işler yapılamaz! 

    Web Store artık 3. versiyon manifestleri kabul etmektedir. 3. versiyonda service worker yapısı gelmiştir. service_worker olarak background.js  tanımlanır. 

    **- "action" :** 
    
        - "default_icon" : Eklentinin ikonu. Bir png dosyasıdır.
        
        - "default_title" : Eklentinin adı 

        - "default_popup" : popup.html dosyasıdır. Aynı isimli olan popup.js dosyasını manifest json içine yazmak gerekmez.

    **- "icons" :** default_icon'dan farklı ikonlar tanımlanabilmesi içindir. 16 favicon'dur. 32 windows için gereklidir. 48 extension sayfası içindir. 128 chrome web store da kullanılır.

    **- "background" :** browser'da gerçekleştirilen işlemleri background ile belirtilmiş service worker'lar ile takip edebilirsiniz. 

        - "service_worker" : service worker olarak çalışan js dosyası yazılır. genelde background.js olarak adlandırılır. Sayfa görünümünü bu dosya ile değiştirmek mümkündür. 
        
        **- "persistent" :** true veya false olabilir. true olursa background script devamlı çalışır. false olursa sadece tetiklendiğinde çalışır. true olursa kaynak kullanımı yüksek olabilir. Bu sebeple parametrik yapılmıştır. false olması genel kabul görmüştür.

    **- "content_scripts" :**
        - "js" :  Manipulate etmek istediğimiz sayfalar için kullanacağımız js dosyalarını belirtiriz. Genelde content.js olarak isimlendirilir. Ama isim önemli değildir.
        - "matches" : Okumak ve değiştirmek istediğimiz sayfalarını yazarız. "Match patterns" olarak aratılabilecek bir konsept ile bu path yazılır.
        - Buraya css dosyası da yazıp, o web sayfasının css lerine de müdahale edebilirsin.

    - manifest.json da content_scripts'in içindeki js dosyası matches bölümünde belirtilen sayfa içeriklerini değiştirme hakkını elde eder. Bu js dosyası content.js dir.

    **- "permissions" :**

        - "scripting" : 
        - "activeTab" : Eklentinin etkinleştirildiği tabda script çalıştırarak bu sekmeyi okuma ve değiştirme izni.
        - "tabs" : Tab'ları açıp kapatabilirsin.Tab'ın başlığı ve url'lerine erişebilirsin.
        - "storage" : Lokal veri depolama izni. 
        - "contextMenus" : Sağ tık yapınca açılan menüye ekleme yapma izni.
        - "notifications" : Push notification gönderme izni.
        - "runtime" : Mesajlaşma izni. 
        - "host_permissions" : Domain adı belirtilmiş sitelere erişim izni. Kullanılması, kullanıcı gizliliğine değer verildiğini gösterir. Extension yüklenirken bu sitelere erişim istendiği bir iletişim kutusu ile belirtilir. Tüm sitelere değil, belirli sitelere erişim istemek iyi olur.
        - "storage" : Diske yazma izni 
        - "omnibox" : "keyword" kelimesi ile yazılan kelimenin adres çubuğuna yazılması ile eklentinin tetiklenmesi izni.
        - "minimum_chrome_version" : Eklenti kurulurken en düşük chrome versiyonunu kontrol eder. 
        - "host_permissions" : Belirtilen host ile iletişim kurma izni. Kurulum sırasında kullanıcıya uyarı gider. API kullanımı içindir. 
        - "optional_permissions" : İsteğe bağlı izinler içindir. Örneğin bunun altındaki "topSites", tarayıcıda en çok ziyaret edilen sitelerin listesine erişim izni verir.
        - "optional_host_permissions" : İsteğe bağlı host erişimleri için izindir. Bunun altında "host_permissions"'ın altına izin verilen hostların ismi yazılır. API kullanımı içindir. 
        - "alarms" : Zamanlanmış görevleri yerine getirebilmesi için gereken izindir.



    - "options_page" : options.html sayfası tanımlanır.

    - "options_ui" : "page" parametresinde options.html tanımlanır. "open_in_tab" parametresinde ise true veya false ile options.html in ayrı bir tabda mı yoksa popup olarak mı açılacağı tanımlanır.

    - "web_accessible_resources" altında options.js tanımlanır. Bu script ile extension'a tanımlanmış özellikler değiştirilebilir. Bu değişiklikleri sağlayacak css ve png türünde dosyalar da gerekliyse bu dosyalar da yine "web_accessible_resources" altında tanımlanır. Bu alan options bölümünde kullanılacak kaynakların tanımlandığı alandır. 

2. POPUP.HTML :
    
    - Açık olan web sayfasının içeriğine doğrudan erişemez. Sayfaya ait "local storage" bilgisine erişemez. Kendisine ait "local storage" bilgisi vardır. 

    - Bu butonlara tıklandığında çalışacak fonksiyonların adları "button" tag'ının içinde "id" olarak yazılır. Bu fonksiyonların bulunduğu js dosyaların adları ise "script" tagının içinde yazılır. Her ikisinin de "body" tagının içinde olması yeterlidir. Fakat kodun okunur olması için alt alta yazılması iyi olur. 

3. POPUP.JS : 
    
    - popup.html ile gösterilen butonlara basılınca çalışır. ayrıca butona basıldığında ne olacağını belirtir. 

4. BACKGROUND.JS : 
    
    - Mesajları dinleyerek ve gerekirse mesaj göndererek arka planda çalışan işlemleri yürütür. 


5. CONTENT.JS : 

    - Butonlara basıldıktan sonra oluşan sonucu sayfaya aktarmak için sayfa içeriğine müdahale etmekle görevlidir. (sayfaya buton ekleme, Div ekleme vb)


ÖNERİLEN KLASÖR YAPISI : 

    - scripts/content.js
    - popup/popup.html, popup.js, popup.css
    - images/tüm icon ve resimler 

MESAJLAŞMANIN KARIŞMAMASI İÇİN : 
    - Gönderici ve alıcı kullan : "sender", "receiver" 
    - Mesaj tipi kullan : "type" 
    - Mesaj ID kullan : "id"

    Mesaj gönderme :
    chrome.runtime.sendMessage({ message: "Merhaba," });

    Mesaj Alma : 
    chrome.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    console.log(request.message);  
  }

