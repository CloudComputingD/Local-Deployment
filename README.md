# Local-Deployment

### Pre Requirements

- **Docker Compose** 설치 되어 있어야 함. 다음 명령어를 통해 버전이 잘 떴다면 설치 된 것.
    - 설치 되지 않았다면, Docker for windows 설치를 제대로 한건지 확인 할 것.
    
    ```bash
    docker-compose -v
    ```
    
- Java17이 설치 되어 있어야 함. 다음 명령어를 통해 버전이 잘 떴다면 설치 된 것.
    - 설치 되지 않았다면, [https://www.oracle.com/java/technologies/downloads/#java17](https://www.oracle.com/java/technologies/downloads/#java17) 에서 설치
    
    ```bash
    java -version
    ```
    
- Postman 설치 되어 있을 거라 믿어요…

### Spring 서버 실행 방법

- jar 파일은 size 이슈로 카톡으로 보내겠음. 압축 풀어서 docker-compose 경로와 같은 경로로(같은 루트) 옮겨줘야 함!
- ![image](https://github.com/CloudComputingD/Local-Deployment/assets/73868703/e74f598c-cfd4-4d0a-8a33-6a5d96b11e13)


- 해당 폴더를 오른쪽 클릭 한 후 [Git Bash here…] 을 클릭 한다.
    - Docker를 통한 DBMS 실행. Spring 서버를 실행 하기 전에 수행 하여야 함.
    
    ```bash
    docker-compose up -d
    ```
    
    - Spring 서버 실행, 종료 시에는 Ctrl + C를 입력 하면 된다.
    
    ```bash
    java -jar filemarket-0.0.1-SNAPSHOT.jar
    ```
    
    - Docker compose 종료 시 (DBMS 메모리 많이 잡아 먹어용)
    
    ```bash
    docker-compose down
    ```

### 서버 실행 후,

- mysql database 컨테이너 bash에 접속
- mysql filemarket -u root -p 를 통해 mysql 접속
- password는 filemarket
- USE filemarket;으로 데이터베이스를 선택 후에
- 아래 sql문으로 테이블들 생성 

```sql
CREATE TABLE `user` (
                         `id`   int   NOT NULL AUTO_INCREMENT PRIMARY KEY ,
                         `name`   varchar(100)   NULL,
                         `password`   varchar(100)   NULL,
                         `email`   varchar(100)   NULL,
                         `role` ENUM('GUEST','USER'),
                         `refresh_token` varchar(255) NULL,
                         `social_id` varchar(32) NULL,
                         `social_type` enum('KAKAO', 'GOOGLE') NULL
);

CREATE TABLE `folder` (
                           `id`   int   NOT NULL AUTO_INCREMENT PRIMARY KEY ,
                           `name`   varchar(128)   NULL,
                           `created_time`   datetime   NULL,
                           `modified_time`   datetime   NULL,
                           `deleted_time`   datetime   NULL,
                           `favorite`   boolean   NOT NULL default 0,
                           `user_id`   int   NOT NULL
);

CREATE TABLE `file_folder` (
                              `id` int NOT NULL AUTO_INCREMENT PRIMARY KEY ,
                              `folder_id` int NOT NULL,
                              `file_id` int Not Null
);

CREATE TABLE `files` (
                         `id`   int   NOT NULL AUTO_INCREMENT PRIMARY KEY ,
                         `name`   varchar(128)   NULL,
                         `created_time`   datetime   NULL,
                         `modified_time`   datetime   NULL,
                         `deleted_time`   datetime   NULL,
                         `extension` varchar(128),
                         `file_size`int,
                         `favorite`   boolean   NOT NULL default 0,
                         `trash`   boolean NOT NULL default 0,
                         `user_id`   int   NOT NULL
);


ALTER TABLE `folder` ADD trash boolean NOT NULL default 0;
```

- [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html) 에 문서 있음.
    - **http://localhost:8080**을 Base URL로 잡고 API 요청 하여 진행 하면 됨.
