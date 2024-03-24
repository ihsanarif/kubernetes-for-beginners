# Kubernetes

## Requirements
* Colima
* Docker
* Kubernetes
* Minikube

## Pengertian Kubernetes atau K8s
Kita lebih sering disebut yaitu containterize + orchestration
```
container + orchestration
```

Nama Kubernetes berasal dari Bahasa Yunani, yang berarti *juru mudi* atau pilot, dan merupakan asal kata gubernur dan cybernetic. K8s merupakan sebuah singkatan yang didapat dengan mengganti 8 huruf "ubernete" dengan "8".


### Kenapa kita perlu `containers`?
* Capability/dependency
* Long setup time
* Different Dev/Test/Prod environments

### Apa yang perlu kita lakukan?
* Apps perlu menjadi containerize
* Jalankan setiap service dengan dependency yg independent atau terpisah setiap pada setiap containers

![concept containers](/images/concept-containerize.png)

### Perbedaan Containers vs Virtual Mechines
![containers vs virtual machines](/images/containers-vs-virtualization.png)

### Keuntungan Containers

Old Principle of deployments
![old deployments](/images/old-principle-deployments.png)

Deployments containerize

![containerize deployments](/images/containerize-deployments.png)


## Prinsip Container Orchstration

Beberapa Orchestration Technologies:
* Docker Swarm
* Kubernetes
* MESOS

Contoh Orchestration Container

![orchestration containers](/images/orchestration-containers.png)

## Install Kubernetes on Local
Saat install kubernetes kita perlu beberapa yang harus diinstall. Pastikan requirement diatas sudah terpenuhi seperti install docker with colima (MacOS M1)

### Install Set Up Kubectl
Berikut install kubectl untuk kebutuhan tools kita disini
```bash
https://kubernetes.io/docs/tasks/tools/
```

Sudah selesai coba pastikan kembali dengan perintah dibawah ini
```bash
➜ kubectl version --client
Client Version: v1.29.2
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```

### Install minikube
Untuk install minikube juga bisa lihat disini
```bash
https://minikube.sigs.k8s.io/docs/start/
```

Jika sudah mengikuti perintah di link diatas untuk install miniku selanjutnya kita coba jalankan minikube-nya dengan pastikan bahwa docker sudah berjalan.

```bash
minikube start --driver=docker
```

Untuk memastikan berjalan `minikube` tersebut kita bisa jalankan perintah dibawah ini
```bash
➜  minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

## Error saat jalankan `minikube`
Masi error? Coba jalanin
```bash
sudo chown -R $USER $HOME/.minikube; chmod -R u+wrx $HOME/.minikube
```
lalu jalankan ini
```bash
minikube start --force-systemd=true
```

## Menjalankan Service pada Kubernetes via Minikube
Kita akan mencoba membuat sebuah Kubernetes Deployments menggunakan image yg sudah ada dengan simple HTTP Server yang di expose 8080.

```bash
➜ kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080

deployment.apps/hello-node created
```

Dan kita coba lihat deployment tersebut dengan perintah
```bash
➜ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
hello-node         1/1     1            1           35m
```

lalu untuk melihat log aktifitas dari server yg sudah kita jalankan
```bash
➜ kubectl logs hello-node-ccf4b9788-bv9wd
I0303 11:31:13.581880       1 log.go:195] Started HTTP server on port 8080
I0303 11:31:13.583262       1 log.go:195] Started UDP server on port  8081
```

Kita buat service dari pod yang sudah dibuat tadi dengan perintah dibawah ini
```bash
➜ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
service/hello-node exposed
```

maka akan terlihat port `8080` berjalan under ports
```bash
➜ kubectl get services
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-node   LoadBalancer   10.98.200.124   <pending>     8080:30890/TCP   110s
```

Untuk mendapatkan URL deployment yang kita jalankan tadi bisa dengan perintah
```bash
➜ minikube service hello-node --url
http://127.0.0.1:60493/
```