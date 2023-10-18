# 요청이 올때 Nginx가 제공을 해줄 Build파일들을 생성하는 단계
FROM node:10 as builder
WORKDIR /app
COPY ./package.json ./
RUN npm install
COPY . .
RUN npm run build

# Nginx를 가동하고 윗단계에서 생성된 빌드 파일들을 제공하여 넘겨준다. 그리고 default.conf에서 해준 설정을 nginx 컨테이너 안에있는 설정이 되게 복사를 해준다.
FROM nginx
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/build /usr/share/nginx/html

# COPY --from=builder /app/build /usr/share/nginx/html
# builder가 생성하면 /app/build에 있는 파일들 컨테이너 안에 /usr/share/nginx/html경로로 복사 


# COPY --from=builder /app/build /usr/share/nginx/html --> 정적 파일을 제공해주기 위한 Nginx를 위해서 frontend안에 nginx 폴더를 생성하고 default.conf 파일 생성 --> front에서 다루는 Nginx는 proxy가 아니라 frontend쪽 Nginx 서버임 --> 프론트쪽 정적파일들 제공해주는 Nginx
# ./nginx/default.conf --> 로컬에 있는 default.conf(nginx 설정파일) 경로
# /etc/nginx/conf.d/default.conf --> 컨테이너 안에 있는 default.conf(nginx 설정파일)


# listen --> nginx 서버가 listen하고 있는 포트
# location --> nginx가 제공해줄 정적 파일들이 있는 위치. location / --> 프론트엔드쪽으로 라우팅 , location /api --> 서버쪽으로

# root /usr/share/nginx/html; --> HTML파일이 위치할 루트 설정
# index index.html index.htm; --> 사이트의 index페이지로 할 파일명 설정

# try_files $uri $uri/ /index.html --> React Router를  사용해서 페이지간 이동을 할때 이 부분이 필요하다.
    # --> React는 Single Page Application 입니다. 그러기에 index.html 하나의 정적 파일만 가지고 있어서 만약 {URL}/home 이렇게 접속을 하려고 할때도
    #     index.html 파일에 접근을 해서 라우팅 시켜야 하는데 nginx에서는 자동으로 이걸 알수가 없습니다. 그러기에 /home에 접속하려고 할때
    #     /home에 매칭되는 것이 없을때에 대안책으로 index.html을 제공하여서 /home으로 라우팅을 시킬수 있게 임의로 설정을 해주는 것