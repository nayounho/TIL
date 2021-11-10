# MQTT 사용 방법

## MQTT 진행과정
- 현재 진행하는 프로젝트에서 Event Log data를 불러와 화면에 렌더링하는 작업을 해야하는 상황이 있다
- Event Log data 전달 과정
  - 설치된 카메라의 영상을 분석하여 분석서버(publish)에서 Topic(Event log data)이 발생하면 Broker로 data 전달
  - 개발서버(local 서버)에서는 subscribe 코드를 설계하여 구독을 하고 있다가 
    Broker에 해당 Topic의 Event log data가 전달되면 그 data를 받아온다

## MQTT 코드 적용 과정

1. MQTT라는 것의 개념이 제대로 잡혀 있지 않은 상태에서 이론만 살펴보고 진행해야 하는 상황이었다. 
   그래서 일단 간단하게 MQTT publish코드 설계와 subscribe 코드를 작성해보기로 하였다
2. 먼저 어떤 MQTT 라이브러리를 사용할 지 고민이 되었다. 그래서 구글링을 통해 js에서 사용할 수 있고 
   대중성이 있는 라이브러리가 2개 있었다
   - paho-mqtt
   - mqtt.js
3. 2개의 라이브러리에 관하여 코드를 각각 작성해 보았다
   1. paho-mqtt
      ```
      import MQTT from 'paho-mqtt';

      const location = {hostname: 'hostname',port: 1338}

      const client = new MQTT.Client(location.hostname, Number(location.port), '/mqtt', '');
      const myTopic = "event/face/recognition"
      const myTopic2 = "event/face/counting"

      client.onConnectionLost = onConnectionLost;
      client.onMessageArrived = onMessageArrived;

      client.connect({ onSuccess: onConnect });
      let count = 0
      ```
    2. mqtt.js
      - publish 코드
        ``` 
        const mqtt = require("mqtt");
        let count = 0;
        const client = mqtt.connect("mqtt://broker.mqtt-dashboard.com", {
          clientId: "vsaas001",
        });
        const options = {
          retain: true,
          qos: 0,
        };
        const topic = "vsaas001";
        let message = "";

        function publish(topic, msg, options) {
          console.log(count);
          if (client.connected == true) {
            console.log("publishing {", topic, " : ", msg, "}");
            client.publish(topic, msg, options);
          }
        }
        const timer_id = setInterval(function () {
          count += 1;
          message = "count " + count;
          publish(topic, message, options);
        }, 4000);
        console.log("end of script");
        ```

       - subscribe 코드
  
        ```
        const mqtt = require("mqtt");
        const client = mqtt.connect("mqtt://broker.mqtt-dashboard.com", {
          clientId: "vsaas100",
        });
        const topic = "vsaas001";

        client.on("message", function (topic, msg, packet) {
          console.log("message :" + msg, topic, packet);
        });

        client.on("connect", function () {
          console.log("connected " + client.connected);
          client.subscribe(topic, { qos: 0 });
        });

        client.on("error", function (error) {
          console.log("Can't connect" + error);
          process.exit(1);
        });

        console.log("subscribing to topics");
        ```
      <img src="./../Image/mqtt%20샘플%20성공.png" width="800px" height="250px" alt="mqtt 샘플 성공"></img>

    - 2개의 라이브러리르 사용해본 결과 paho-mqtt는 중간에 설계과정에서 실패하였다. 
      docs에 나와있는 대로 따라하면서 진행하였는데 결과가 잘 나오지 않았었다. 짐작해보면 
      borker의 문제 인것으로 생각되지만 mqtt.js가 실행에 성공하여 더이상 깊게 보지 않았다
      mqtt.js는 내가 publish에서 메세지를 넘겨주면 subscribe에서 잘 받아왔다
      그래서 이 코드를 바탕으로 프로젝트에 적용하려고 하였다. 그런데...
    - 응용만 하면 되는 것인데 결과가 나오지 않거나 에러가 발생하였다.
  
      <img src="./../Image/mqtt%20에러.png" width="450px" height="200px" alt="mqtt 에러"></img>

    - 이틀을 삽질했는데 구글 검색에서는 실제 적용되는 코드는 별로 건질게 없었다. 믿을건 docs뿐...
      한가지 건진건 React에 적용된 mqtt subscribe코드를 찾을 수 있었다.
      <a href="https://www.preciouschicken.com/blog/posts/a-taste-of-mqtt-in-react/">참고 사이트</a>

       - React적용 mqtt subscribe 코드
  
        ```
        import React, {useState} from 'react';

        const mqtt = require('mqtt');

        const options = {
          protocol: 'mqtt',
          clientId: 'vsaas01'
        };

        const client = mqtt.connect('mqtt://broker.mqtt-dashboard.com', options);

        client.subscribe('vsaas001')

        const App = () => {
          const [msg, setMsg] = useState(<span>nothing heard</span>)

        client.on('message', function(topic, message) {
          let note = message.toString();
          setMsg(note);
          console.log(topic, note);
          client.end();
        })

          return (
            <div>
              <header>
                <h1>Test MQTT</h1>
                <span>The message is : {msg}</span>
              </header>
            </div>
          )
        }

        export default App;
        ```

      - 하지만... 역시 한번에 될리가 없지...ㅎㅎ
        broker에 구독하는 연결이 안되었다. connected fail... connected fail... 후...
      - 문제가 무엇인지 계속 보고 또 보고 문제가 없는 것처럼 느껴졌다. broker가 문제라는 결론을 내고
        AI분석 개발자님에게 broker적용하는 것만 문의를 드렸다. 
        분석 개발자님도 React에서 사용하는 mqtt subscribe 코드는 잘 몰랐지만 java로 코드를 짜서 시연을 보여주셨다
        그 과정에서 connect에 연결하는 도메인을 현재 실행되고 있는 도메인으로 바꿨더니 OMG!! 
        결과를 전달 받을 수 있었다. 드디어 성공!!

      - 문제의 코드
        ```const client = mqtt.connect('mqtt://broker.mqtt-dashboard.com', options);```
        수정 후
        ```const client = mqtt.connect('mqtt://사내도메인', options);```

        짜잔~

        <img src="./../Image/mqtt-react%20성공.png" width="750px" height="600px" alt="mqtt-react 성공"></img>