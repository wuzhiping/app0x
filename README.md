# app0x
app0x - with sidecar


## :3500/v1.0/invoke/app0x/method/model/act
## :3500/model/act [with header dapr-app-id: app0x]

curl -H "dapr-app-id:app0x" -H "Content-Type: application/json" -X POST -d '[{"key":"key","value":1}]' http://localhost:3500/v1.0/state/statestore
<pre>
  app0x:
    build: ./app0x
    image: app0x
    restart: always
    command: /bin/bash ./start.sh
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone:ro
      - ./app0x/C4/:/opt/bpm/C4/
    environment:
      - VER=0.0.1
      - TZ=Asia/Shanghai
    depends_on:
      - redisdb
      - placement
    ports:
      #- "3303:3303"
      - "3500:3500"
      #- "50000:50000"
  app0x-dapr:
    image: "daprio/daprd:edge-linux-arm"
    command: ["./daprd",
     "-app-id", "app0x",
     "-app-port", "3303",
     "-dapr-grpc-port", "50000",
     "-placement-host-address", "placement:50005",
     "-components-path", "/dapr/components",
     "-config", "/dapr/config.yaml"]
    volumes:
        - "./dapr/:/dapr/"
    depends_on:
      - app0x
    network_mode: "service:app0x"

</pre>
