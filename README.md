# GCP_실습 1

## Cloud Storage

### Web을 Cloud Storage 파일 관리

- GCP Console 로 접속 (https://console.cloud.google.com/)

- GCP project 생성 및 선택

![project_setting](./images/project_setting.jpg)

- Cloud Storage로 이동

![cloud_storage](./images/cloud_storage.jpg)


- 버킷 이름 만들기
  * 버킷 버전 관리 활성화


- 버킷에 파일 업로드

- 객체 확인

- 같은 객체를 다시 업로드

- 버전 확인하기

![second_version](./images/second_version.jpg)


### GSutil 을 이용한 Cloud Storage 

- Windows terminal을 열어, 우분투로 진입한 뒤, GCP CLI 설치

- 최신 배포판(Debian 9+ 또는 Ubuntu 18.04+)의 경우 다음 명령어를 실행

```
sudo curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
    
```

- 패키지 소스로 gcloud CLI 배포 URI를 추가 

```
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
```


- gcloud CLI를 업데이트하고 설치

```
sudo apt-get update && sudo apt-get install google-cloud-cli
```


- GCP 계정 정보를 기반으로 권한 획득

```
gcloud init
```

- gstuil version 확인

```
gsutil version -l
```

- 새 버킷 만들기

```
gsutil mb gs://BUCKET_NAME
```

- 버킷에 객체 업로드 

```
gsutil cp OBJECT_LOCATION gs://DESTINATION_BUCKET_NAME/
```
- 버킷 삭제

```
gsutil rm -r gs://BUCKET_NAME
```


## Artifact Registry 실습

- Artifact Registry 저장소 만들기


- docker desktop을 수행시킨뒤, 레지스트리에 대한 권한 인증


```
gcloud auth configure-docker asia-northeast3-docker.pkg.dev
```


- 만든 저장소의 레파지토리 복사

![registry.](./images/registry.jpg)

- nginx 도커 이미지를 공식 도커 허브에서 가져오기

```
docker pull busybox
```

- GCP registry 를 위한 도커 이미지 태깅

![registry](./images/registry.jpg)

```
docker tag busybox asia-northeast3-docker.pkg.dev/cuk-practice/docker-registry/busybox
```

- docker push

```
docker push asia-northeast3-docker.pkg.dev/cuk-practice/docker-registry/busybox
```


## GKE 실습

### GKE Autopilot을 이용한 k8s 클러스터 실습

- Autopilot 기반으로 GKE 생성
  * 배포되는 pod의 개수를 기반으로 컴퓨팅 리소스가 탄력적으로 증감

- web 을 통한 컨테이너 배포

![autopilot-gke](./images/autopilot-gke.jpg)


- docker desktop에서 kubernetes를 수행시킨다. 

- 우분투상에서 GKE 인증 플러그인을 설치한다. 


```
sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin

```

- kubectl context를 생성된 GKE 클러스터로부터 가져온다. 

```
gcloud container clusters get-credentials autopilot-cluster-1 --region asia-northeast3 --project cuk-practice
```


- 생성된 클러스터의 컨텍스트가 정상적으로 설정되었는지 확인한다. 

```
sudo kubectl get namespaces
```