<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Konum Bilgileri</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
            text-align: center;
            padding-top: 50px;
        }

        button {
            background-color: #3498db;
            color: white;
            padding: 15px 30px;
            border: none;
            font-size: 18px;
            cursor: pointer;
            border-radius: 5px;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #2980b9;
        }

        #location-info {
            display: none;
            margin-top: 30px;
            padding: 20px;
            background-color: #ecf0f1;
            border-radius: 5px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            width: 60%;
            margin-left: auto;
            margin-right: auto;
            font-size: 16px;
            text-align: left;
            color: #333;
        }

        h2 {
            color: #3498db;
        }
    </style>
</head>
<body>

<h1>Konum Bilgileri</h1>
<p>Konumunuzu görmek için aşağıdaki butona tıklayın.</p>

<!-- Konum Bilgilerini Gösteren Buton -->
<button onclick="getLocation()">Konum Bilgilerini Görüntüle</button>

<!-- Konum Bilgileri İçeriği -->
<div id="location-info">
    <h2>Konum Bilgileriniz</h2>
    <p><strong>Yer:</strong> <span id="location-name"></span></p>
</div>

<script>
    function getLocation() {
        // Kullanıcının konumunu almak için geolocation API'sini kullanıyoruz
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(showPosition, showError);
        } else {
            alert("Geolocation API desteklenmiyor.");
        }
    }

    // Konum bilgilerini ekranda gösterme
    function showPosition(position) {
        var latitude = position.coords.latitude;  // Enlem
        var longitude = position.coords.longitude;  // Boylam

        // OpenStreetMap Nominatim API'yi kullanarak koordinatları yer adına dönüştürme
        var apiUrl = `https://nominatim.openstreetmap.org/reverse?lat=${latitude}&lon=${longitude}&format=json`;

        // API'ye istek gönderme
        fetch(apiUrl)
            .then(response => response.json())
            .then(data => {
                var locationName = data.display_name;  // Yer adı
                document.getElementById("location-name").innerText = locationName;
                var locationInfoDiv = document.getElementById("location-info");
                locationInfoDiv.style.display = "block";
            })
            .catch(error => {
                alert("Konum bilgileri alınırken bir hata oluştu.");
            });
    }

    // Hata durumunda kullanıcıya mesaj gösterme
    function showError(error) {
        switch(error.code) {
            case error.PERMISSION_DENIED:
                alert("Kullanıcı konum paylaşımını reddetti.");
                break;
            case error.POSITION_UNAVAILABLE:
                alert("Konum bilgisi mevcut değil.");
                break;
            case error.TIMEOUT:
                alert("Konum alımı zaman aşımına uğradı.");
                break;
            case error.UNKNOWN_ERROR:
                alert("Bilinmeyen bir hata oluştu.");
                break;
        }
    }
</script>

</body>
</html>
