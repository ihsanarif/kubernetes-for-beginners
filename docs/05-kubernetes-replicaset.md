# Introduction Kubernetes Replica Controller & Replicaset

## Replica Controller
Replica Controller adalah mekanisme yang digunakan untuk melakukan replikasi aplikasi dan kita bisa handle ketika aplikasi kita terdapat *failure* atau melakukan inisialisasi POD yang *failed*. Maka dengan Replica controller ini kita bisa mengatur scale suatu aplikasi kita yang sedang berjalan.

Misalkan, ketika satu *failed* maka aplikasi lain yang sedang berjalan akan tetap berjalan diatur oleh **Replica Controller** pada Cluster Kubernetes.

Lalu ada yang disebut **Replica Controller** dan ada juga **Replica Set**, apa perbedaannya? Replica Set adalah jalan untuk melakukan atau membuat konfigurasi *replication*.

> The difference between a **replica set** and a **replication controller** is that a replica set supports set-based selector requirements whereas a replication controller only supports equality-based selector requirements.

## Membuat ReplicaSet
Setelah kita tahu istilah-istilah tersebut, selanjutnya kita akan mencoba mengimplementasikan-nya.

Pertama kita buat YAML file `rc-definition.yaml` lalu isi file tersebut dengan kode dibawah ini.
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
```

Kita terapkan **Replica Controller** tersebut dengan perintah dibawh ini.
```bash
➜ kubectl create -f code/replicasets/rc-definition.yaml
replicationcontroller/myapp-rc created
```

Kita bisa lihat Replica kita sudah berjalan atau tidak dengan perintah dibawah ini.
```bash
➜ kubectl get rc
NAME       DESIRED   CURRENT   READY   AGE
myapp-rc   3         3         3       28s
```

Dari tampilan dibawah ini maka akan terlihat ada 3 PODs yang sedang berjalan. Untuk memastikan POD yang berjalan sesuai maka bisa kita lihat dengan perintah dibawah ini.
```bash
➜ kubectl get pods
NAME             READY   STATUS    RESTARTS   AGE
myapp-rc-j5zdv   1/1     Running   0          2m52s
myapp-rc-pxqcb   1/1     Running   0          2m52s
myapp-rc-rrcrj   1/1     Running   0          2m52s
```

Terlihat ada 3 PODs yang sedang berjalan dengan nama prefix `myapp-rc` dan sisanya di generate oleh sistem.


## Membuat Replica Set
Kita buat YAML file `replicaset-definition.yaml` lalu isi file tersebut dengan kode dibawah ini.
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

Terapkan *ReplicaSet* pada file tersebut dengan perintah
```bash
➜ kubectl create -f code/replicasets/replicaset-definition.yaml 
replicaset.apps/myapp-replicaset created
```

lalu lihat apakah sudah berjalan atau tidak dengan cara perintah ini.
```bash
➜ kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
myapp-replicaset   3         3         3       41s
```

Dan kita lihat juga PODs yang sedang berjalan dengan perintah ini.
```bash
➜ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
myapp-replicaset-2cglx   1/1     Running   0          80s
myapp-replicaset-mb4z8   1/1     Running   0          80s
myapp-replicaset-rrqc2   1/1     Running   0          80s
```

Jika kita ingin `Scale` ReplicaSet tersebut ada 2 cara yaitu
1. **Scale** dengan edit YAML file misalkan ingin `Scale` menjadi 6 maka kita ubah seperti ini.
    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
    name: myapp-replicaset
    labels:
        app: myapp
        type: front-end
    spec:
    template:
        metadata:
        name: myapp-pod
        labels:
            app: myapp
            type: front-end
        spec:
        containers:
            - name: nginx-container
            image: nginx
    replicas: 6
    selector:
        matchLabels:
        type: front-end
    ```

    Lalu jalankan perintah ini
    ```bash
    ➜  kubectl replace -f code/replicasets/replicaset-definition.yaml
    replicaset.apps/myapp-replicaset replaced
    ```

    Lihat ReplicaSet dan Pods denga perintah
    ```bash
    ➜ kubectl get replicaset,pod
    NAME                               DESIRED   CURRENT   READY   AGE
    replicaset.apps/myapp-replicaset   6         6         6       5m55s

    NAME                         READY   STATUS    RESTARTS   AGE
    pod/myapp-replicaset-2cglx   1/1     Running   0          5m55s
    pod/myapp-replicaset-h8w9d   1/1     Running   0          101s
    pod/myapp-replicaset-mb4z8   1/1     Running   0          5m55s
    pod/myapp-replicaset-ppr52   1/1     Running   0          101s
    pod/myapp-replicaset-rbhlp   1/1     Running   0          101s
    pod/myapp-replicaset-rrqc2   1/1     Running   0          5m55s
    ```

    Bisa terlihat diatas PODs sudah berubah jumlahnya menjadi 6.

2. **Scale** secara langsung dengan perintah terminal
    ```bash
    ➜  kubectl scale --replicas=8 -f code/replicasets/replicaset-definition.yaml 
    replicaset.apps/myapp-replicaset scaled
    ```

    Maka akan terlihat pada replicaset dan PODs dengan jumlah 8
    ```bash
    ➜ kubectl get replicaset,pod                                            
    NAME                               DESIRED   CURRENT   READY   AGE
    replicaset.apps/myapp-replicaset   8         8         8       8m40s

    NAME                         READY   STATUS    RESTARTS   AGE
    pod/myapp-replicaset-2cglx   1/1     Running   0          8m40s
    pod/myapp-replicaset-h8w9d   1/1     Running   0          4m26s
    pod/myapp-replicaset-lxfrf   1/1     Running   0          22s
    pod/myapp-replicaset-mb4z8   1/1     Running   0          8m40s
    pod/myapp-replicaset-ppr52   1/1     Running   0          4m26s
    pod/myapp-replicaset-rbhlp   1/1     Running   0          4m26s
    pod/myapp-replicaset-rrqc2   1/1     Running   0          8m40s
    pod/myapp-replicaset-znr7n   1/1     Running   0          22s
    ```

    atau bisa pakai alternatif perintah terminal seperti ini
    ```bash
    ➜  kubectl scale --replicas=8 replicaset myapp-replicaset
    ```

Uniknya, dengan *ReplicaSet* ini, jika kita menghapus salah satu pods yang ada maka nanti akan di scale ulang PODs sampai memenuhi scale yang sudah ditentukan, misalkan kita sudah punya `myapp-relicaset` dengan jumlah 8 PODs. Kita coba hapus 2 PODs dengan perintah ini
```bash
➜  kubernetes-for-beginners git:(main) ✗ kubectl delete pod myapp-replicaset-2cglx myapp-replicaset-h8w9d
pod "myapp-replicaset-2cglx" deleted
pod "myapp-replicaset-h8w9d" deleted
```

Maka akan terlihat PODs baru yang akan berjalan kita bisa melihatnya dengan perintah
```bash
➜  kubectl get replicaset,pod                   
NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/myapp-replicaset   8         8         7       25m

NAME                         READY   STATUS              RESTARTS   AGE
pod/myapp-replicaset-l568n   1/1     Running             0          33s
pod/myapp-replicaset-lxfrf   1/1     Running             0          17m
pod/myapp-replicaset-mb4z8   1/1     Running             0          25m
pod/myapp-replicaset-ppr52   1/1     Running             0          21m
pod/myapp-replicaset-rbhlp   1/1     Running             0          21m
pod/myapp-replicaset-rrqc2   1/1     Running             0          25m
pod/myapp-replicaset-t9pg5   0/1     ContainerCreating   0          2s
pod/myapp-replicaset-znr7n   1/1     Running             0          17m
```

Bisa kita lihat ada PODs yang memiliki status `ContainerCreating` tanda bahwa POD ini baru dibuat.