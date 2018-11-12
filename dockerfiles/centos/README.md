# Dockerfiles for WSO2 Enterprise Integrator #


Esta seção define as instruções passo-a-passo para construir imagens do Docker baseadas em Linux para vários perfis [CentOS] (https://hub.docker.com/_/centos/)
fornecido pelo WSO2 Enterprise Integrator 6.4.0, a saber: <br>

1. Integrator
2. Micro integrator
3. Business Process
4. Broker
5. MSF4J
6. Analytics Dashboard
7. Analytics Worker

## Prerequisitos
* [Docker](https://www.docker.com/get-docker) v17.09.0 or above

## Como construir uma imagem e executar

##### 1. Checkout este repositório em sua máquina local usando o seguinte comando git.

```
git clone https://github.com/wso2/docker-ei.git
```

>A cópia local do diretório `dockerfiles / centos` será referida como` DOCKERFILE_HOME` deste ponto em diante

##### 2. Adicione a distribuição JDK, WSO2 Enterprise Integrator e as bibliotecas necessárias.

- Download [JDK 1.8]
extraia em `<DOCKERFILE_HOME>/base/files`.
- Download [WSO2 Enterprise Integrator 6.4.0](https://wso2.com/integration/) e extraia
em `<DOCKERFILE_HOME>/base/files`.
- Depois que a distribuição do JDK e do WSO2 Enterprise Integrator for extraída, ela deverá ser a seguinte:
```
<DOCKERFILE_HOME>/base/files/jdk<version>
<DOCKERFILE_HOME>/base/files/wso2ei-6.4.0
```
- Download [MySQL Connector/J](https://downloads.mysql.com/archives/c-j/) v5.1.45 e copie para `<DOCKERFILE_HOME>/base/files`,`<DOCKERFILE_HOME>/analytics/dashboard/files`and `<DOCKERFILE_HOME>/analytics/worker/files` folder
- Download [Andes Client](http://maven.wso2.org/nexus/content/groups/wso2-public/org/wso2/andes/wso2/andes-client/3.2.82/) JAR v3.2.82,
[Geronimo JMS Spec](http://maven.wso2.org/nexus/content/groups/wso2-public/org/apache/geronimo/specs/wso2/geronimo-jms_1.1_spec/1.1.0.wso2v1/) JAR v1.1.0.wso2v1 and
[Secure-vault](http://maven.wso2.org/nexus/content/groups/wso2-public/org/wso2/securevault/org.wso2.securevault/1.0.0-wso2v2/) JAR v.1.0.0-wso2v2 <br> to 
`<DOCKERFILE_HOME>/integrator/files`.
Essas bibliotecas são necessárias para a comunicação entre o Integrator <br> e o Message Broker.
- Se você pretende usar as imagens do Docker com implantações de produtos em cluster do Kubernetes, baixe o
[Kubernetes membership scheme](http://central.maven.org/maven2/org/wso2/carbon/kubernetes/artifacts/kubernetes-membership-scheme/1.0.5/kubernetes-membership-scheme-1.0.5.jar)
and [dnsjava for DNS lookups](http://central.maven.org/maven2/dnsjava/dnsjava/2.1.8/dnsjava-2.1.8.jar) o Jar deve ser copiadao para o folder
`<DOCKERFILE_HOME>/base/files` .

>Por favor, consulte[WSO2 Update Manager documentation]( https://docs.wso2.com/display/WUM300/WSO2+Update+Manager)
a fim de obter as últimas correções de bugs e atualizações para o produto.

##### 3. Build the base Docker image.
- For base, navigate to `<DOCKERFILE_HOME>/base` directory. <br>
  Execute `docker build` command as shown below.
    + `docker build -t wso2ei-base:6.4.0-centos .`
        
##### 4. Build Docker images specific to each profile.
- For Integrator, navigate to `<DOCKERFILE_HOME>/integrator` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-integrator:6.4.0-centos .`
- For Micro Integrator, navigate to `<DOCKERFILE_HOME>/micro-integrator` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-micro-integrator:6.4.0-centos .`        
- For Business Process Server, navigate to `<DOCKERFILE_HOME>/business-process` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-business-process:6.4.0-centos .`
- For Message Broker, navigate to `<DOCKERFILE_HOME>/broker` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-broker:6.4.0-centos .`
- For MSF4J, navigate to `<DOCKERFILE_HOME>/msf4j` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-msf4j:6.4.0-centos .`
- For Analytics Dashboard, navigate to `<DOCKERFILE_HOME>/analytics/dashboard` directory. <br>
  Execute `docker build` command as shown below. 
    + `docker build -t wso2ei-analytics-dashboard:6.4.0-centos .`
- For Analytics Worker, navigate to `<DOCKERFILE_HOME>/analytics/worker` directory. <br>
   Execute `docker build` command as shown below. 
     + `docker build -t wso2ei-analytics-worker:6.4.0-centos .`
    
##### 5. Running docker images specific to each profile.
- For Integrator,
    + `docker run -p 8280:8280 -p 8243:8243 -p 9443:9443 wso2ei-integrator:6.4.0-centos`
- For Micro Integrator,
    + `docker run -p 8290:8290 -p 8253:8253 wso2ei-micro-integrator:6.4.0-centos`
- For Business Process Server,
    + `docker run -p 9445:9445  -p 9765:9765 wso2ei-business-process:6.4.0-centos`  
- For Message Broker,
    + `docker run -p 9446:9446 -p 5675:5675 ...all-port-mappings-here... wso2ei-broker:6.4.0-centos` 
- For MSF4J,
    + `docker run -p 9090:9090 wso2ei-msf4j:6.4.0-centos`
- For Analytics Dashboard,
    + `docker run -p 9713:9713 -p 9643:9643 ...all-port-mappings-here... wso2ei-analytics-dashboard:6.4.0-centos`
- for Analytics Worker,
    + `docker run -p 9444:9444 ...all-port-mappings-here... wso2ei-analytics-worker:6.4.0-centos`

##### 6. Accessing management console per each profile.
- For Integrator,
    + `https:<DOCKER_HOST>:9443/carbon`
- For Business Process Server,
    + `https:<DOCKER_HOST>:9445/carbon`
- For Message Broker,
    + `https:<DOCKER_HOST>:9446/carbon`
- For Analytics Dashboard,
    + Business Rules:<br>
    `https://<DOCKER_HOST>:9643/business-rules`
    + Portal:<br>
    `https://<DOCKER_HOST>:9643/portal`
    + Monitoring:<br>
    `https://<DOCKER_HOST>:9643/monitoring`
    
>In here, <DOCKER_HOST> refers to hostname or IP of the host machine on top of which containers are spawned.

## Como atualizar configurações
As configurações estariam na máquina host do Docker e podem ser montadas em volume no contêiner. <br>
Como exemplo, as etapas necessárias para alterar o deslocamento da porta do perfil do integrador usando `carbon.xml` são as seguintes.

##### 1. Pare o contêiner do integrador se ele já estiver em execução.
Na distribuição de produtos do WSO2 Enterprise Integrator 6.4.0, o arquivo carbon.xml para o perfil do integrador <br>
pode ser encontrado em `<DISTRIBUTION_HOME> / conf`. Copie o arquivo para algum local adequado da máquina host, <br>
referido como `<SOURCE_CONFIGS> / carbon.xml` e altere o valor do deslocamento em portas para 1.

##### 2. Conceder permissão de leitura aos usuários `other` para` <SOURCE_CONFIGS> / carbon.xml`
`` `
chmod o + r <SOURCE_CONFIGS> /carbon.xml
`` `

##### 3. Execute a imagem montando o arquivo no container da seguinte maneira.
docker run 
-p 8281:8281 -p 8244:8244 -p 9444:9444
--volume <SOURCE_CONFIGS>/carbon.xml:<TARGET_CONFIGS>/carbon.xml
wso2ei-integrator:6.4.0-centos
```

>In here, <TARGET_CONFIGS> refers to /home/wso2carbon/wso2ei-6.4.0/conf folder of the container.

## Docker command usage references

* [Docker build command reference](https://docs.docker.com/engine/reference/commandline/build/)
* [Docker run command reference](https://docs.docker.com/engine/reference/run/)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
