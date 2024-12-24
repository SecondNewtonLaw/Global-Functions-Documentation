# WebSocket

The **WebSocket** class provides a simple interface for sending and receiving data over a WebSocket connection.

---

## WebSocket.connect

Establishes a WebSocket connection to the specified URL.

```luau
WebSocket.connect(url: string): WebSocketConnection
```

### Parameters

- `url`: The URL of the WebSocket server to connect to.

### Example

```luau
local ws = WebSocket.connect("wss://echo.websocket.org")

if ws then
    print("WebSocket connection established.")
else
    warn("Failed to connect to WebSocket server.")
end
```

---

## WebSocketConnection.Connected

A boolean which indicates whether the Connection is still online.

```luau
WebSocketConnection.Connected: boolean
```

### Example

```luau
print(ws.Connected) -- prints "true" since we havent closed the connection yet
```

---

## WebSocketConnection.Send

Sends data over the WebSocket connection.

```luau
WebSocketConnection:Send(data: string): nil
```

### Parameters

- `data`: The data to send.

### Example

```luau
ws:Send("Hello, server!")
```

---

## WebSocketConnection.Close

Closes the WebSocket connection.

```luau
WebSocketConnection:Close(): nil
```

### Example

```luau
ws:Close()
```

---

## WebSocketConnection.OnMessage

This event is fired when a message is received from the server.

```luau
WebSocketConnection.OnMessage: RBXScriptSignal<string>
```

### Arguments

- `message`: The message which was sent from the connection.

### Example

```luau
ws.OnMessage:Connect(function(message: string)
    print("Received message:", message)
end)
```

---

## WebSocketConnection.OnClose

This event is fired when the WebSocket connection is closed.

```luau
WebSocketConnection.OnClose: RBXScriptSignal
```

### Example

```luau
ws.OnClose:Connect(function()
    print("Websocket connection has closed!")
end)
```
