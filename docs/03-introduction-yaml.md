# Introduction YAML


## YAML Configuration
**A YAML** file is used to represent data. This case configuration data here is a quick comparison of sample data in three different formats.

The one on the left is XML where we display a list of servers and their information the same data is

represented in Json format in the middle and finally in YAML format to the right.

## Contoh Penggunaan YAML
YAML adalah struktur yang merepresentasikan data dari suatu konfigurasi yang dibuat. misalkan konfigurasi suatu server-server dengan detail:
* nama: Server1
* owner: Ihsan
* created: 12122024
* status: active

maka implementasi ke dalam YAML file seperti ini
```yaml
servers:
    - name: Server1
      owner: Ihsan
      created: 12122024
      status: active
```

Dapat bisa kita lihat bahwa tanda `-` ini menandakan array yang memiliki fields `name`, `owner`, `created` dan `status`. 

Sedangkan jika kita konversikan menjadi JSON dari file YAML tersebut akan seperti ini.
```json
{
    "servers":[
        {
            "name": "Server1",
            "owner": "Ihsan",
            "created": 12122024,
            "status": "active"
        }
    ]
}
```

Lalu jika kita ingin menambahkan array value saja dari suatu `key` maka kita bisa tambahkan misalkan seperti ini.
```yaml
servers:
    - name: Server1
      owner: Ihsan
      created: 12122024
      status: active
      labels:
        - backend
        - services
        - apps
```

dan jika kita konversikan ke dalam file JSON akan seperti dibawah ini.
```json
{
    "servers":[
        {
            "name": "Server1",
            "owner": "Ihsan",
            "created": 12122024,
            "status": "active",
            "labels": [
                "backend",
                "services",
                "apps"
            ]
        }
    ]
}
```

Untuk mempelajarinya terkait format file YAML sangat mudah hanya saja perlu banyak latihan untuk membuat konfigurasi ini menggunakan file YAML.

Hal-hal yang perlu diperhatikan dari file YAML
* Indentasi dari setiap baris itu menentukan dari suatu object `parent child`-nya.
* Jika kita membuat `array` value dari yaml maka itu akan terbaca secara berurutan jika di ubah urutannya maka akan berubah juga urutan eksekusinya.
* Jika kita ingin membuat komentar bisa menggunakan tanda `#` diawal baris.

## Kenapa kita mempelajari YAML File
Pada konfigurasi kubernetes ini banyak file yang dibuat menggunakan format YAML, sehingga kita perlu hafal dan paham bagaimana mekanisme dan pembuatan format file menggunakan YAML. Sehingga nanti pada saat kita membuat konfigurasi `service` kubernetes kita tidak kesusahan untuk mencerna dan memahami konfigurasi atau struktur dari file YAML yang sudah kita buat ini.
