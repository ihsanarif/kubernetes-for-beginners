# Introduction Kubernetes Pods

Kita akan mencoba membuat `pod` menggunakan YAML file pada Kubernetes. Kubernetes biasanya menggunakan YAML file untuk membuat suatu objek-objek konfigurasi terkait PODs, replica, deployment, service dan lainnya.

Struktur dari YAML file pada Kubernetes itu secara umum memiliki kesamaan dalam 4 level fields paling atas yaitu:
```yaml
apiVersion:
kind:
metadata:


spec:
```

4 fields level ini pasti ada saat kita membuat konfigurasi YAML pada Kubernetes.

Berikut ini beberapa `Kind` ada perbedaan dalam pengisian `apiVersion`.

| Kind          | apiVersion    |
|---------------|---------------|
| POD           | v1            |
| Service       | v1            |
| ReplicaSet    | apps/v1       |
| Deployment    | apps/v1       |

Sehingga ketika kita ingin membuat suatu POD menggunakan YAML file maka akan seperti dibawah ini.
```yaml
apiVersion: v1
kind: Pod
metadata:


spec:
```

Selanjutnya kita akan mencoba membuat file dengan nama `pod-definition.yaml` file pada folder `code/pod/pod-definition.yaml` lalu isi YAML file tersebut seperti dibawah ini.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: backend
spec:
  containers:
    - name: nginx-container
      image: nginx
```

Berikut ini definisi dari setiap fields:
* `metadata` ini berisikan `name` dan `labels` yang mana ini bisa kita isi sesuai dengan kebutuhannya yang mana pengisiannya pun dengan struktur `key` dan `value`.
* `spec` ini kita set list `containers` yang akan kita buat dalam satu POD tersebut. Pada file tersebut kita hanya satu container saja yang diset dengan `name` nginx-container dengan `image` nginx.

Setelah selesai file tersebut dibuat maka kita coba eksekusi file tersebut dengan perintah dibawah ini.
```bash
➜ kubectl apply -f code/pod/pod-definition.yaml 
pod/myapp-pod created
```

Lalu kita lihat apakah sudah terbuat dengan perintah
```bash
➜ kubectl get pods                             
NAME        READY   STATUS              RESTARTS   AGE
myapp-pod   0/1     ContainerCreating   0          3s
```

Jika kita ingin melihat lebih detail POD yang sudah kita buat bisa dengan perintah dibawah ini
```bash
➜ kubectl describe pod myapp-pod
Name:             myapp-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Sun, 03 Mar 2024 20:19:23 +0700
Labels:           app=myapp
                  type=backend
Annotations:      <none>
Status:           Running
IP:               10.244.0.23
IPs:
  IP:  10.244.0.23
Containers:
  nginx-container:
    Container ID:   docker://fdb7680e1a75ab77bdf200d4c17086af2a6604e18d22abc5ea2e26258d80775f
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:c26ae7472d624ba1fafd296e73cecc4f93f853088e6a9c13c0d52f6ca5865107
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sun, 03 Mar 2024 20:19:26 +0700
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-bfkcl (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-bfkcl:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  64s   default-scheduler  Successfully assigned default/myapp-pod to minikube
  Normal  Pulling    65s   kubelet            Pulling image "nginx"
  Normal  Pulled     62s   kubelet            Successfully pulled image "nginx" in 3.201s (3.201s including waiting)
  Normal  Created    62s   kubelet            Created container nginx-container
  Normal  Started    62s   kubelet            Started container nginx-container
```

Informasi detail dari PODs diatas sangat lengkap sekali sampai kita bisa melihat event dari PODs tersebut ketika ada perubahan yang terjadi pada POD tersebut.