Proje Hakkında

Bu Python script'i, web uygulamalarında kapsamlı bir keşif yaparak, potansiyel Sunucu Taraflı İstek Sahteciliği (SSRF) zafiyetlerini tespit etmeyi amaçlar. Temel olarak, hedef bir ana alan adı üzerinde alt alan adı ve dizin taraması gerçekleştirir. Keşfedilen her geçerli URL'yi daha derinlemesine analiz eder ve içerisindeki bağlantıları çıkarır. Çıkarılan bu bağlantılara HTTP isteği göndererek, bir webhook üzerinden yanıtın başarılı olup olmadığını kontrol eder. Bu yöntem, bir sunucunun, bir saldırganın kontrolündeki harici bir kaynağa (webhook'a) istek gönderip göndermediğini dolaylı yoldan tespit etmek için kullanılır, bu da potansiyel bir SSRF göstergesidir.
Amaç ve Hedef Kitle

Bu projenin temel amacı, otomatik keşif süreçlerini SSRF zafiyet tespiti ile birleştirerek güvenlik araştırmacılarına ve sızma testi uzmanlarına yardımcı olmaktır. Özellikle şu kitlelere hitap eder:

    Güvenlik Araştırmacıları: Hedef ağlardaki görünür varlıkları (alt alan adları, dizinler) keşfetmek ve bu süreçte olası SSRF zafiyetlerini tespit etmek için.
    Sızma Testleri Uzmanları: Kapsamlı keşif ve SSRF kontrolünü tek bir araçta birleştirmek isteyenler.
    Bug Bounty Avcıları: Potansiyel SSRF bulgularını hızlıca belirleyip kendi kontrol mekanizmalarına (webhook) göndermek isteyenler.

Özellikler

    Alt Alan Adı Taraması: Verilen bir alt alan adı listesi dosyasını kullanarak hedef ana alan adı için aktif alt alan adlarını bulur.
    Dizin Taraması: Verilen bir dizin listesi dosyasını kullanarak hedef ana alan adı veya keşfedilen alt alan adları üzerinde dizinleri keşfeder.
    SSRF Tespiti: Keşfedilen her URL'deki tüm bağlantıları çıkarır ve bu bağlantılara istek gönderir. Eğer webhook bağlantısı başarılı bir şekilde çağrılırsa, potansiyel bir SSRF zafiyeti olduğuna dair uyarı verir.
    Webhook Entegrasyonu: Bulguları, kullanıcı tarafından sağlanan bir webhook URL'sine göndererek dış sistemlere bildirim sağlar. Bu, özellikle kontrolünüzdeki bir sunucuya istek gelip gelmediğini izlemek için kullanışlıdır.
    Renkli Çıktı: Kullanıcıya daha iyi bir görsel geri bildirim sağlamak için colorama kütüphanesi ile renkli terminal çıktıları sunar.
    URL Ayrıştırma ve Birleştirme: urllib.parse modülünü kullanarak URL'leri doğru bir şekilde ayrıştırır ve birleştirir.

Gereklilikler

Bu aracı kullanmak için sisteminizde aşağıdaki yazılımların kurulu olması gerekir:

    Python 3.x: Script Python 3 ile uyumludur.
    requests kütüphanesi: HTTP istekleri yapmak için. pip install requests komutu ile kurulabilir.
    colorama kütüphanesi: Terminal çıktılarını renklendirmek için. pip install colorama komutu ile kurulabilir.
    BeautifulSoup4 kütüphanesi: HTML içeriğini ayrıştırmak ve bağlantıları çıkarmak için. pip install beautifulsoup4 komutu ile kurulabilir.

Kurulum ve Kullanım

    Gerekli Kütüphaneleri Kurun:
    Script'i çalıştırmadan önce, Python ortamınızda gerekli kütüphanelerin kurulu olduğundan emin olun:
    Bash

pip install requests colorama beautifulsoup4

Script'i İndirin:
Bu projenin GitHub deposundan ssrf_scanner.py (veya script'inizin adı ne ise) dosyasını indirin veya kopyalayın.

Listeleri Hazırlayın:

    Alt Alan Adı Listesi: Taramak istediğiniz alt alan adlarını her satıra bir tane gelecek şekilde bir metin dosyasına kaydedin (örneğin: subdomains.txt).

    admin
    dev
    test
    api
    blog

    Dizin Listesi: Taramak istediğiniz dizinleri her satıra bir tane gelecek şekilde bir metin dosyasına kaydedin (örneğin: directories.txt).

    admin/
    login/
    test/
    .git/HEAD
    robots.txt

Webhook Hazırlayın:
Bir webhook URL'si edinin. Bu, genellikle istekleri alıp kaydedebileceğiniz özel bir URL'dir (örneğin, webhook.site gibi ücretsiz servisler veya kendi kontrolünüzdeki bir sunucuya kuracağınız basit bir dinleyici). Bu webhook URL'sine bir istek gelip gelmediğini kontrol ederek SSRF zafiyetinin varlığını doğrulayacaksınız.

Script'i Çalıştırın:
Terminalinizde script'in bulunduğu dizine gidin ve aşağıdaki komutu çalıştırın. Script sizden hedef ana alan adını, alt alan adı listesi dosyasını, dizin listesi dosyasını ve webhook URL'sini isteyecektir.
Bash

    python ssrf_scanner.py

    İstendiğinde bilgileri girin:

    Ana domaini girin (örn: example.com): target.com
    Subdomain listesi dosyasının adını girin: subdomains.txt
    Directory listesi dosyasının adını girin: directories.txt
    Webhook linkini girin: https://webhook.site/YOUR_UNIQUE_WEBHOOK_URL

Bulguları Değerlendirme

Tarama tamamlandığında, terminalde keşfedilen alt alan adlarını ve dizinleri göreceksiniz. Eğer bir URL'deki harici bir bağlantıya yapılan isteğin ardından sizin belirttiğiniz webhook URL'sine bir istek gelirse (bu isteği webhook.site gibi servislerde veya kendi sunucunuzun loglarında kontrol etmelisiniz), bu bir potansiyel SSRF zafiyetinin göstergesidir.

[+] Potansiyel SSF Zafiyeti Bulundu: [link] mesajı, script'in belirtilen link adresine yaptığı bir istek sonucunda webhook'unuza bir geri bildirim aldığını gösterir. Bu, hedef sunucunun kontrolünüzdeki bir URL'ye istek atabildiği anlamına gelir ve ciddi bir güvenlik açığıdır.

Önemli Not: SSRF tespiti, genellikle, hedef sunucunun kontrolünüzdeki bir dış kaynağa (bu durumda webhook'unuza) istek gönderme yeteneğini doğrulamaya dayanır. Script'in çıktısıyla birlikte webhook'unuzun loglarını mutlaka kontrol edin ve hangi IP adresinden, hangi User-Agent ile istek geldiğini inceleyin. Bu, zafiyetin gerçekliğini ve potansiyel etkisini teyit etmenin en önemli adımıdır.
Önemli Etik Not

Bu araç, web uygulamalarındaki güvenlik zafiyetlerini tespit etmek için tasarlanmıştır. Bu tür araçların yalnızca yasal ve etik sınırlar içinde kullanılması büyük önem taşımaktadır. Hedef sistemler üzerinde test yapmadan önce kesinlikle sahibinden yazılı izin almalısınız. İzinsiz tarama veya sömürü girişimleri yasa dışıdır ve ciddi hukuki sonuçları olabilir. Bu aracın kötüye kullanımıyla ilgili herhangi bir sorumluluk kabul edilmez.
Geliştirme Önerileri

Bu script, SSRF araştırmalarında güçlü bir başlangıç noktası sunar. Gelecekteki geliştirmeler için bazı fikirler:

    Protokol Çeşitliliği: Yalnızca HTTP/HTTPS değil, file://, gopher://, dict:// gibi diğer protokolleri de test edebilen SSRF payload'ları ekleyin.
    Parametre Enjeksiyonu: SSRF payload'larını yalnızca çıkarılan bağlantılara değil, URL'deki sorgu parametrelerine veya POST isteklerinin gövdelerine de enjekte etme yeteneği ekleyin.
    Zafiyet Doğrulama Mekanizması: Webhook yerine, SSRF zafiyetinin doğruluğunu kanıtlayacak, dahili IP'leri tarama, dosya okuma (LFI ile birlikte) gibi daha agresif ama kontrollü payload'lar ekleyin.
    Error Handling (Hata Yönetimi): Network hataları, timeout'lar ve diğer istisnalar için daha sağlam hata yönetimi ekleyin.
    Proxy Desteği: İsteğe bağlı olarak proxy (örneğin Burp Suite) üzerinden trafik gönderme yeteneği ekleyerek manuel analiz ve debug imkanı sağlayın.
    Kullanıcı Arayüzü/Raporlama: Bulguları daha yapılandırılmış bir şekilde (JSON, HTML) raporlama veya basit bir web arayüzü sunma.
    Konkurrent İstekler: asyncio ve aiohttp gibi kütüphanelerle asenkron istekler yaparak tarama hızını artırın.

Katkıda Bulunma

Proje daha fazla geliştirmeye açık! Yeni özellikler eklemek, zafiyet tespit mantığını iyileştirmek, hata yönetimi geliştirmeleri yapmak veya yeni özellikler önermek isterseniz, geri bildirimleriniz, hata raporlarınız ve katkılarınız her zaman açığız. Bir çekme isteği (pull request) göndermeden önce lütfen mevcut sorunları kontrol edin veya yeni bir sorun açın.
Lisans

Bu proje MIT Lisansı altında yayınlanmıştır. Daha fazla bilgi için 'LICENSE' dosyasına bakın.
İletişim

Sorularınız, önerileriniz veya işbirliği talepleriniz için bana github.com/0batexe1 üzerinden ulaşabilirsiniz.

English Version

Webhook-Integrated SSRF & Discovery Scanner
About The Project

This Python script aims to identify potential Server-Side Request Forgery (SSRF) vulnerabilities by performing comprehensive reconnaissance on web applications. It primarily conducts subdomain and directory scanning on a given main domain. For each valid URL discovered, it performs a deeper analysis by extracting internal and external links. It then sends HTTP requests to these extracted links and, if a successful response is received via a webhook, it signals a potential SSRF vulnerability. This method is used to indirectly detect whether a server is making requests to an external resource controlled by an attacker (the webhook), which is indicative of a potential SSRF.
Purpose and Target Audience

The primary goal of this project is to assist security researchers and penetration testers by combining automated discovery processes with SSRF vulnerability detection. It specifically targets the following audiences:

    Security Researchers: For discovering visible assets (subdomains, directories) within target networks and identifying potential SSRF vulnerabilities in the process.
    Penetration Testers: Those looking to combine comprehensive discovery and SSRF checks into a single tool.
    Bug Bounty Hunters: Those who want to quickly identify potential SSRF findings and report them to their own control mechanisms (webhooks).

Features

    Subdomain Scanning: Discovers active subdomains for a given main domain using a provided subdomain list file.
    Directory Scanning: Discovers directories on the main domain or discovered subdomains using a provided directory list file.
    SSRF Detection: Extracts all links from each discovered URL and sends requests to these links. If the webhook link is successfully called, it issues a warning about a potential SSRF vulnerability.
    Webhook Integration: Provides notification to external systems by sending findings to a user-provided webhook URL. This is particularly useful for monitoring whether a request has arrived at a server you control.
    Colored Output: Provides colored terminal output using the colorama library for better visual feedback to the user.
    URL Parsing and Joining: Correctly parses and joins URLs using the urllib.parse module.

Requirements

To use this tool, the following software must be installed on your system:

    Python 3.x: The script is compatible with Python 3.
    requests library: For making HTTP requests. Can be installed with pip install requests.
    colorama library: For coloring terminal output. Can be installed with pip install colorama.
    BeautifulSoup4 library: For parsing HTML content and extracting links. Can be installed with pip install beautifulsoup4.

Installation and Usage

    Install Required Libraries:
    Before running the script, ensure that the necessary libraries are installed in your Python environment:
    Bash

pip install requests colorama beautifulsoup4

Download the Script:
Download or copy the ssrf_scanner.py file (or whatever your script is named) from this project's GitHub repository.

Prepare Your Lists:

    Subdomain List: Save the subdomains you want to scan in a text file, with one subdomain per line (e.g., subdomains.txt).

    admin
    dev
    test
    api
    blog

    Directory List: Save the directories you want to scan in a text file, with one directory per line (e.g., directories.txt).

    admin/
    login/
    test/
    .git/HEAD
    robots.txt

Prepare Your Webhook:
Obtain a webhook URL. This is typically a special URL where you can receive and log requests (e.g., free services like webhook.site or a simple listener you set up on your own server). You will verify the existence of the SSRF vulnerability by checking if a request arrives at this webhook URL.

Run the Script:
Navigate to the directory where you saved the script in your terminal and run the following command. The script will prompt you for the target main domain, the subdomain list file, the directory list file, and the webhook URL.
Bash

    python ssrf_scanner.py

    Enter the information when prompted:

    Ana domaini girin (örn: example.com): target.com
    Subdomain listesi dosyasının adını girin: subdomains.txt
    Directory listesi dosyasının adını girin: directories.txt
    Webhook linkini girin: https://webhook.site/YOUR_UNIQUE_WEBHOOK_URL

Evaluating Findings

Once the scan is complete, you will see the discovered subdomains and directories in your terminal. If, after a request to an external link within a URL, you receive a request at your specified webhook URL (you should check this in the logs of services like webhook.site or on your own server), this is an indicator of a potential SSRF vulnerability.

The message [+] Potansiyel SSF Zafiyeti Bulundu: [link] indicates that the script received feedback at your webhook after making a request to the specified link address. This means the target server was able to make a request to a URL you control, which is a serious security flaw.

Important Note: SSRF detection typically relies on verifying the target server's ability to send a request to an external resource you control (in this case, your webhook). Always check your webhook's logs in conjunction with the script's output, and examine the IP address and User-Agent from which the request originated. This is the most crucial step for confirming the authenticity and potential impact of the vulnerability.
Important Ethical Note

This tool is designed to identify security vulnerabilities in web applications. It is of utmost importance that such tools are used strictly within legal and ethical boundaries. You must obtain explicit written permission from the owner before conducting any tests on target systems. Unauthorized scanning or exploitation attempts are illegal and can lead to severe legal consequences. No responsibility is assumed for any misuse of this tool.
Improvement Suggestions

This script provides a strong starting point for SSRF research. Here are some ideas for future enhancements:

    Protocol Variety: Add SSRF payloads that test other protocols beyond just HTTP/HTTPS, such as file://, gopher://, dict://.
    Parameter Injection: Add the ability to inject SSRF payloads not only into extracted links but also into query parameters in the URL or within the bodies of POST requests.
    Vulnerability Verification Mechanism: Instead of just a webhook, add more aggressive but controlled payloads to prove the SSRF vulnerability's validity, such as scanning internal IPs or reading files (in conjunction with LFI).
    Error Handling: Implement more robust error handling for network errors, timeouts, and other exceptions.
    Proxy Support: Add the ability to optionally send traffic through a proxy (e.g., Burp Suite) for manual analysis and debugging.
    User Interface/Reporting: Structured reporting capabilities (JSON, HTML) or a simple web interface.
    Concurrent Requests: Increase scanning speed by using asynchronous requests with libraries like asyncio and aiohttp.

Contributing

The project is open for further development! If you'd like to add new features, improve vulnerability detection logic, enhance error handling, or propose new features, your feedback, bug reports, and contributions are always welcome. Please check for existing issues or open a new one before submitting a pull request.
License

This project is licensed under the MIT License. See the 'LICENSE' file for more details.
Contact

For any questions, suggestions, or collaboration inquiries, feel free to reach out to me via github.com/0batexe1.
