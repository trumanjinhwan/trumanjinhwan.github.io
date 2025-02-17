---
layout: post
title: "3. 웹소켓을 활용한 실시간 데이터 처리"
date: 2025-01-27
---

> 프로젝트를 진행하면서 **다른 클라이언트가 추가, 수정, 삭제한 작업들이 새로고침 없이 실시간으로 반영될 수 있을까?** 라는 고민을 하게 되었습니다. 검색과 실험을 거쳐 **웹소켓(WebSocket)**을 활용하면 가능하다는 사실을 알게 되었고, 이에 대한 내용을 정리해보겠습니다.

# 웹소켓이란?

<div style="text-align: center;">
    <img src="/사진들/웹소켓/HTTPvs웹소켓.png" alt="alt text" />
</div>
>출처: <https://www.scaleway.com/en/blog/iot-hub-what-use-case-for-websockets/>

<br>

HTTP는 클라이언트가 요청해야 서버가 응답하는 **비연결지향적, 단방향 통신** 방식입니다. 반면, 웹소켓은 **연결을 유지하며 양방향 통신**을 지원하므로, 클라이언트 요청 없이도 서버가 데이터를 보낼 수 있습니다. 이를 활용하면 실시간 데이터 처리가 가능해집니다. 예를 들어, 다른 사용자가 데이터를 변경하면 서버에서 즉시 갱신된 데이터를 모든 클라이언트에 전송할 수 있습니다.

---

# 웹소켓 사용법

<br>

<div style="text-align: center;">
    <img src="/사진들/웹소켓/톰켓 내장 API.png" alt="alt text" />
</div>

<br>

아파치 톰캣 9버전 이상에서는 내장 웹소켓 API(`websocket-api.jar`)를 지원합니다.

위 API를 활용하면 별도의 라이브러리 없이도 웹소켓을 구현할 수 있습니다.

---

# 웹소켓 서버 구현 예제

```java
package webSocket;

import java.io.IOException;
import java.util.*;
import javax.websocket.*;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import dao.*; // TaskDAO 가져오기

@javax.websocket.server.ServerEndpoint("/ws")
public class WebSocketServer {
    private static Map<Session, String> sessionDashboardMap = new HashMap<>();
    private static Set<Session> sessions = new HashSet<>();

    @OnOpen
    public void onOpen(Session session) {
        System.out.println("새로운 연결: " + session.getId());
        sessions.add(session);
    }

    @OnMessage
    public void onMessage(Session session, String message) {
        System.out.println("받은 메시지: " + message);
        try {
            JSONObject jsonMessage = (JSONObject) new org.json.simple.parser.JSONParser().parse(message);
            String type = (String) jsonMessage.get("type");
            String dashboardId = String.valueOf(jsonMessage.get("dashboardId"));

            if ("update_dashboard".equals(type)) {
                sessionDashboardMap.put(session, dashboardId);
                sendUpdatedTasksToAll(dashboardId);
            } else if ("update_request".equals(type)) {
                sendUpdatedTasksToAll(dashboardId);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @OnClose
    public void onClose(Session session) {
        System.out.println("연결 종료: " + session.getId());
        sessions.remove(session);
        sessionDashboardMap.remove(session);
    }

    @OnError
    public void onError(Session session, Throwable throwable) {
        throwable.printStackTrace();
    }

    private void sendUpdatedTasksToAll(String dashboardId) {
        JSONArray taskList = new TaskDAO().getDashboardTasks(dashboardId);
        JSONObject message = new JSONObject();
        message.put("type", "update");
        message.put("tasks", taskList);
        message.put("dashboardId", dashboardId);

        for (Session session : sessions) {
            if (dashboardId.equals(sessionDashboardMap.get(session))) {
                try {
                    session.getBasicRemote().sendText(message.toJSONString());
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 코드 설명

- `@javax.websocket.server.ServerEndpoint("/ws")`: 클라이언트가 접속할 웹소켓 엔드포인트를 설정합니다.
- `@OnOpen`: 새로운 연결이 생기면 세션을 저장합니다.
- `@OnMessage`: 클라이언트로부터 메시지를 받아 처리하고 변경 사항을 전송합니다.
- `@OnClose`: 세션이 종료되면 목록에서 제거합니다.
- `sendUpdatedTasksToAll()`: 특정 대시보드에 접속한 모든 클라이언트에게 업데이트된 작업 목록을 전송합니다.

---

# 클라이언트에서 웹소켓 활용하기

```html
<script>
var socket = new WebSocket('ws://34.138.23.151:8080/Project4team_PMS/ws');

socket.onopen = function() {
    console.log("WebSocket 연결됨");
    if (dashboardId !== "") {
        updateDashboardOnWebSocket();
    }
};

socket.onmessage = function(event) {
    try {
        let message = JSON.parse(event.data);
        console.log("서버 응답:", message);
        if (message.type === "update" && message.tasks) {
            if (message.dashboardId === String(dashboardId)) {
                populateTable(message.tasks);
            }
        }
    } catch (e) {
        console.error("잘못된 메시지 형식: ", event.data);
    }
};

function requestTaskUpdate() {
    var message = {
        type: "update_request",
        dashboardId: String(dashboardId)
    };
    socket.send(JSON.stringify(message));
}

function fetchTasks() {
    var url = "jsp/getTask.jsp";
    var params = { dashboardId: dashboardId };

    AJAX.call(url, params, function (data) {
        var tasks = JSON.parse(data);
        populateTable(tasks);
        if (socket.readyState === WebSocket.OPEN) {
            requestTaskUpdate();
        } else {
            console.log("WebSocket이 아직 연결되지 않음.");
        }
    });
}
</script>
```

## 코드 설명

- `socket.onopen`: 서버와의 연결이 완료되면 실행됩니다.
- `socket.onmessage`: 서버에서 데이터를 수신하면 실행됩니다.
- `requestTaskUpdate()`: 서버에 최신 작업 목록을 요청합니다.
- `fetchTasks()`: AJAX와 웹소켓을 조합하여 데이터를 불러옵니다.

---

# 결론

웹소켓을 활용하면 HTTP 요청 없이도 실시간 데이터 동기화가 가능합니다. 이를 통해 작업 변경 사항을 즉각적으로 반영하고, 불필요한 요청을 줄여 성능을 향상시킬 수 있습니다. 웹소켓은 특히 협업 기반 애플리케이션(ex PMS)에서 필수적인 기술로 자리 잡고 있으며, 적절한 예외 처리 및 보안 설정과 함께 적용하는 것이 중요합니다.