# Tips and Trics Developing Kubernetes on Visual Studio Code

Ketika kita sedang membuat YAML file dengan menggunakan Visual Studio Code, ada Plugin/Extension yang bisa mempermudah sehingga kita tidak terjadi kesalahan pada pembuatan YAML file dan juga akan dibantu dengan *auto suggestion*.

Berikut kamu perlu install di Visual Studio Code-nya dibawah ini
[YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml).

Setelah diinstall kita perlu tambahan konfigurasi pada Visual Studio Code-nya dengan Pilih **Settings** 

![edit settings](/images/trick-tips-1.png) 

![save config JSON](/images/trick-tips-2.png)

File pada `settings.json` seperti dibawah ini
```json
{
    "yaml.schemas": {
        "kubernetes": "*.yaml"
    }
}
```

Jika file tersebut sudah disimpan maka ketika kita buat YAML file dengan key Kubernetes akan terlihat *auto suggestion* seperti dibawah ini.

![suggestion api version](/images/yaml-suggestion-2.png)
Auto Suggestion apiVersion

![suggestion kind](/images/yaml-suggesstion-1.png) 
Auto Suggestion kind

![alt text](/images/yaml-info.png)
Information fields