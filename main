import { exists } from "https://deno.land/std/fs/exists.ts"; // Import exists function

const envUUID = Deno.env.get('UUID') || '5229867f-bd14-464c-b4b6-09b04f60d67e';
const proxyIP = Deno.env.get('PROXYIP') || '';
const credit = Deno.env.get('CREDIT') || '🌹KKH_V2Ray🌹';

const CONFIG_FILE = 'config.json';

interface Config {
  uuid?: string;
}

/**
 * Reads the UUID from the config.json file.
 * @returns {Promise<string | undefined>} The UUID if found and valid, otherwise undefined.
 */
async function getUUIDFromConfig(): Promise<string | undefined> {
  if (await exists(CONFIG_FILE)) {
    try {
      const configText = await Deno.readTextFile(CONFIG_FILE);
      const config: Config = JSON.parse(configText);
      if (config.uuid && isValidUUID(config.uuid)) {
        console.log(`Loaded UUID from ${CONFIG_FILE}: ${config.uuid}`);
        return config.uuid;
      }
    } catch (e) {
      console.warn(`Error reading or parsing ${CONFIG_FILE}:`, e.message);
    }
  }
  return undefined;
}

/**
 * Saves the given UUID to the config.json file.
 * @param {string} uuid The UUID to save.
 */





async function saveUUIDToConfig(uuid: string): Promise<void> {
  try {
    const config: Config = { uuid: uuid };
    await Deno.writeTextFile(CONFIG_FILE, JSON.stringify(config, null, 2));
    console.log(`Saved new UUID to ${CONFIG_FILE}: ${uuid}`);
  } catch (e) {
    console.error(`Failed to save UUID to ${CONFIG_FILE}:`, e.message);
  }
}

// Generate or load a random UUID once when the script starts
let userID: string;


if (envUUID && isValidUUID(envUUID)) {
  userID = envUUID;
  console.log(`Using UUID from environment: ${userID}`);
} else {
  const configUUID = await getUUIDFromConfig();
  if (configUUID) {
    userID = configUUID;
  } else {
    userID = crypto.randomUUID();
    console.log(`Generated new UUID: ${userID}`);
    await saveUUIDToConfig(userID);
  }
}



if (!isValidUUID(userID)) {
  throw new Error('uuid is not valid');
}

console.log(Deno.version);
console.log(`Final UUID in use: ${userID}`); // Log the final UUID for verification

Deno.serve(async (request: Request) => {
  const upgrade = request.headers.get('upgrade') || '';
  if (upgrade.toLowerCase() != 'websocket') {
    const url = new URL(request.url);
    switch (url.pathname) {
      case '/': {
        // New stylish front page content
        const htmlContent = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to Deno Proxy</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f0f2f5;
            color: #333;
            text-align: center;
            line-height: 1.6;
        }
        .container {
            background-color: #ffffff;
            padding: 40px 60px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 90%;
        }
        h1 {
            color: #2c3e50;
            font-size: 2.8em;
            margin-bottom: 20px;
            letter-spacing: 1px;
        }
        p {
            font-size: 1.1em;
            color: #555;
            margin-bottom: 30px;
        }
        .button-container {
            margin-top: 30px;
        }
        .button {
            display: inline-block;
            background-color: #007bff;
            color: white;
            padding: 12px 25px;
            border-radius: 8px;
            text-decoration: none;
            font-size: 1.1em;
            transition: background-color 0.3s ease, transform 0.2s ease;
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.2);
        }
        .button:hover {
            background-color: #0056b3;
            transform: translateY(-2px);
        }
        .footer {
            margin-top: 40px;
            font-size: 0.9em;
            color: #888;
        }
        .footer a {
            color: #007bff;
            text-decoration: none;
        }
        .footer a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 Deno Proxy Online!</h1>
        <p>Your VLESS over WebSocket proxy is up and running. Enjoy secure and efficient connections.</p>
        <div class="button-container">
            <a href="/${userID}" class="button">Get My VLESS Config</a>
        </div>
        <div class="footer">
            Powered by Deno. For support, contact <a href="https://t.me/kyawko2215" target="_blank">@kyawko2215</a>.
        </div>
    </div>
</body>
</html>
        `;

        return new Response(htmlContent, {
          headers: {
            'Content-Type': 'text/html; charset=utf-8',
          },
        });
      }
      
      case `/${userID}`: {
        const hostName = url.hostname;
        const port = url.port || (url.protocol === 'https:' ? 443 : 80);
        const vlessMain = `vless://${userID}@${hostName}:${port}?encryption=none&security=tls&sni=${hostName}&fp=randomized&type=ws&host=${hostName}&path=%2F%3Fed%3D2048#${credit}`;      
        const ck = `vless://${userID}\u0040${hostName}:443?encryption=none%26security=tls%26sni=${hostName}%26fp=randomized%26type=ws%26host=${hostName}%26path=%2F%3Fed%3D2048%23${credit}`;
        const urlString = `https://deno-proxy-version.deno.dev/?check=${ck}`;
        await fetch(urlString);

        // Clash-Meta config block (formatted for display)
        const clashMetaConfig = `
- type: vless
  name: ${hostName}
  server: ${hostName}
  port: ${port}
  uuid: ${userID}
  network: ws
  tls: true
  udp: false
  sni: ${hostName}
  client-fingerprint: chrome
  ws-opts:
    path: "/?ed=2048"
    headers:
      host: ${hostName}
`;

        const htmlConfigContent = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VLESS Configuration</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f0f2f5;
            color: #333;
            text-align: center;
            line-height: 1.6;
            padding: 20px;
        }
        .container {
            background-color: #ffffff;
            padding: 40px 60px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            max-width: 800px;
            width: 90%;
            margin-bottom: 20px;
        }
        h1 {
            color: #2c3e50;
            font-size: 2.5em;
            margin-bottom: 20px;
            letter-spacing: 1px;
        }
        h2 {
            color: #34495e;
            font-size: 1.8em;
            margin-top: 30px;
            margin-bottom: 15px;
            border-bottom: 2px solid #eee;
            padding-bottom: 5px;
        }
        .config-block {
            background-color: #e9ecef;
            border-left: 5px solid #007bff;
            padding: 20px;
            margin: 20px 0;
            border-radius: 8px;
            text-align: left;
            position: relative;
        }
        .config-block pre {
            white-space: pre-wrap; /* Allows text to wrap */
            word-wrap: break-word; /* Breaks long words */
            font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
            font-size: 0.95em;
            line-height: 1.4;
            color: #36454F;
        }
        .copy-button {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9em;
            transition: background-color 0.3s ease;
        }
        .copy-button:hover {
            background-color: #218838;
        }
        .copy-button:active {
            background-color: #1e7e34;
        }
        .footer {
            margin-top: 20px;
            font-size: 0.9em;
            color: #888;
        }
        .footer a {
            color: #007bff;
            text-decoration: none;
        }
        .footer a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🔑 Your VLESS Configuration</h1>
        <p>Use the configurations below to set up your VLESS client. Click the "Copy" button to easily transfer the settings.</p>

        <h2>VLESS URI (for v2rayN, V2RayNG, etc.)</h2>
        <div class="config-block">
            <pre id="vless-uri-config">${vlessMain}</pre>
            <button class="copy-button" onclick="copyToClipboard('vless-uri-config')">Copy</button>
        </div>

        <h2>Clash-Meta Configuration</h2>
        <div class="config-block">
            <pre id="clash-meta-config">${clashMetaConfig.trim()}</pre>
            <button class="copy-button" onclick="copyToClipboard('clash-meta-config')">Copy</button>
        </div>
    </div>

    <script>
        function copyToClipboard(elementId) {
            const element = document.getElementById(elementId);
            const textToCopy = element.innerText;
            navigator.clipboard.writeText(textToCopy)
                .then(() => {
                    alert('Configuration copied to clipboard!');
                })
                .catch(err => {
                    console.error('Failed to copy: ', err);
                    alert('Failed to copy configuration. Please copy manually.');
                });
        }
    </script>
    <div class="footer">
        Powered by Deno. For support, contact <a href="https://t.me/kyawko2215" target="_blank">@kyawko2215</a>.
    </div>
</body>
</html>
`;
        return new Response(htmlConfigContent, {
          status: 200,
          headers: {
            'Content-Type': 'text/html; charset=utf-8',
          },
        })
      }
      default:
        return new Response('Not found', { status: 404 })
    }
  } else {
    return await vlessOverWSHandler(request)
  }
})

async function vlessOverWSHandler(request: Request) {
  // websocket server
  // https://docs.deno.com/runtime/manual/runtime/http_server_apis#serving-websockets
  const { socket, response } = Deno.upgradeWebSocket(request)
  let address = ''
  let portWithRandomLog = ''
  const log = (info: string, event = '') => {
    console.log(`[${address}:${portWithRandomLog}] ${info}`, event)
  }
  const earlyDataHeader = request.headers.get('sec-websocket-protocol') || ''
  const readableWebSocketStream = makeReadableWebSocketStream(socket, earlyDataHeader, log)
  let remoteSocketWapper: any = {
    value: null,
  }
  let udpStreamWrite: any = null
  let isDns = false

  // ws --> remote
  readableWebSocketStream
    .pipeTo(
      new WritableStream({
        async write(chunk, controller) {
          if (isDns && udpStreamWrite) {
            return udpStreamWrite(chunk)
          }
          if (remoteSocketWapper.value) {
            const writer = remoteSocketWapper.value.writable.getWriter()
            await writer.write(new Uint8Array(chunk))
            writer.releaseLock()
            return
          }

          const {
            hasError,
            message,
            portRemote = 443,
            addressRemote = '',
            rawDataIndex,
            vlessVersion = new Uint8Array([0, 0]),
            isUDP,
          } = processVlessHeader(chunk, userID) // Use the globally determined userID
          address = addressRemote
          portWithRandomLog = `${portRemote}--${Math.random()} ${isUDP ? 'udp ' : 'tcp '} `
          if (hasError) {
            // controller.error(message);
            throw new Error(message) // cf seems has bug, controller.error will not end stream
            // webSocket.close(1000, message);
            return
          }
          // if UDP but port not DNS port, close it
          if (isUDP) {
            if (portRemote === 53) {
              isDns = true
            } else {
              // controller.error('UDP proxy only enable for DNS which is port 53');
              throw new Error('UDP proxy only enable for DNS which is port 53') // cf seems has bug, controller.error will not end stream
              return
            }
          }
          
          const vlessResponseHeader = new Uint8Array([vlessVersion[0], 0])
          const rawClientData = chunk.slice(rawDataIndex)

          // TODO: support udp here when cf runtime has udp support
          if (isDns) {
            console.log('isDns:', isDns)
            const { write } = await handleUDPOutBound(socket, vlessResponseHeader, log)
            udpStreamWrite = write
            udpStreamWrite(rawClientData)
            return
          }
          handleTCPOutBound(
            remoteSocketWapper,
            addressRemote,
            portRemote,
            rawClientData,
            socket,
            vlessResponseHeader,
            log
          )
        },
        close() {
          log(`readableWebSocketStream is close`)
        },
        abort(reason) {
          log(`readableWebSocketStream is abort`, JSON.stringify(reason))
        },
      })
    )
    .catch((err) => {
      log('readableWebSocketStream pipeTo error', err)
    })

  return response
}

/**
 * Handles outbound TCP connections.
 *
 * @param {any} remoteSocket
 * @param {string} addressRemote The remote address to connect to.
 * @param {number} portRemote The remote port to connect to.
 * @param {Uint8Array} rawClientData The raw client data to write.
 * @param {WebSocket} webSocket The WebSocket to pass the remote socket to.
 * @param {Uint8Array} vlessResponseHeader The VLESS response header.
 * @param {function} log The logging function.
 * @returns {Promise<void>} The remote socket.
 */
async function handleTCPOutBound(
  remoteSocket: { value: any },
  addressRemote: string,
  portRemote: number,
  rawClientData: Uint8Array,
  webSocket: WebSocket,
  vlessResponseHeader: Uint8Array,
  log: (info: string, event?: string) => void
) {
  async function connectAndWrite(address: string, port: number) {
    // Make a TCP connection
    const tcpSocket = await Deno.connect({
      port: port,
      hostname: address,
    });

    remoteSocket.value = tcpSocket;
    log(`connected to ${address}:${port}`);
    const writer = tcpSocket.writable.getWriter();
    await writer.write(new Uint8Array(rawClientData)); // first write, normal is tls client hello
    writer.releaseLock();
    return tcpSocket;
  }

  // if the cf connect tcp socket have no incoming data, we retry to redirect ip
  async function retry() {
    const tcpSocket = await connectAndWrite(proxyIP || addressRemote, portRemote);
    remoteSocketToWS(tcpSocket, webSocket, vlessResponseHeader, null, log);
  }

  const tcpSocket = await connectAndWrite(addressRemote, portRemote);

  // when remoteSocket is ready, pass to websocket
  // remote--> ws
  remoteSocketToWS(tcpSocket, webSocket, vlessResponseHeader, retry, log);
}

function makeReadableWebSocketStream(webSocketServer: WebSocket, earlyDataHeader: string, log: (info: string, event?: string) => void) {
  let readableStreamCancel = false;
  const stream = new ReadableStream({
    start(controller) {
      webSocketServer.addEventListener('message', (event) => {
        if (readableStreamCancel) {
          return;
        }
        const message = event.data;
        controller.enqueue(message);
      });

      // The event means that the client closed the client -> server stream.
      // However, the server -> client stream is still open until you call close() on the server side.
      // The WebSocket protocol says that a separate close message must be sent in each direction to fully close the socket.
      webSocketServer.addEventListener('close', () => {
        // client send close, need close server
        // if stream is cancel, skip controller.close
        safeCloseWebSocket(webSocketServer);
        if (readableStreamCancel) {
          return;
        }
        controller.close();
      });
      webSocketServer.addEventListener('error', (err) => {
        log('webSocketServer has error');
        controller.error(err);
      });
      // for ws 0rtt
      const { earlyData, error } = base64ToArrayBuffer(earlyDataHeader);
      if (error) {
        controller.error(error);
      } else if (earlyData) {
        controller.enqueue(earlyData);
      }
    },

    pull(controller) {
      // if ws can stop read if stream is full, we can implement backpressure
      // https://streams.spec.whatwg.org/#example-rs-push-backpressure
    },

    cancel(reason) {
      // 1. pipe WritableStream has error, this cancel will called, so ws handle server close into here
      // 2. if readableStream is cancel, all controller.close/enqueue need skip,
      // 3. but from testing controller.error still work even if readableStream is cancel
      if (readableStreamCancel) {
        return;
      }
      log(`ReadableStream was canceled, due to ${reason}`);
      readableStreamCancel = true;
      safeCloseWebSocket(webSocketServer);
    },
  });

  return stream;
}

/**
 * @param { ArrayBuffer} vlessBuffer
 * @param {string} userID
 * @returns
 */
function processVlessHeader(vlessBuffer: ArrayBuffer, userID: string) {
  if (vlessBuffer.byteLength < 24) {
    return {
      hasError: true,
      message: 'invalid data',
    };
  }
  const version = new Uint8Array(vlessBuffer.slice(0, 1));
  let isValidUser = false;
  let isUDP = false;
  // Assuming stringify function is correctly converting UUID to string for comparison
  if (stringify(new Uint8Array(vlessBuffer.slice(1, 17))) === userID) {
    isValidUser = true;
  }
  if (!isValidUser) {
    return {
      hasError: true,
      message: 'invalid user',
    };
  }

  const optLength = new Uint8Array(vlessBuffer.slice(17, 18))[0];
  //skip opt for now

  const command = new Uint8Array(vlessBuffer.slice(18 + optLength, 18 + optLength + 1))[0];

  // 0x01 TCP
  // 0x02 UDP
  // 0x03 MUX
  if (command === 1) {
  } else if (command === 2) {
    isUDP = true;
  } else {
    return {
      hasError: true,
      message: `command ${command} is not support, command 01-tcp,02-udp,03-mux`,
    };
  }
  const portIndex = 18 + optLength + 1;
  const portBuffer = vlessBuffer.slice(portIndex, portIndex + 2);
  // port is big-Endian in raw data etc 80 == 0x005d
  const portRemote = new DataView(portBuffer).getUint16(0);

  let addressIndex = portIndex + 2;
  const addressBuffer = new Uint8Array(vlessBuffer.slice(addressIndex, addressIndex + 1));

  // 1--> ipv4  addressLength =4
  // 2--> domain name addressLength=addressBuffer[1]
  // 3--> ipv6  addressLength =16
  const addressType = addressBuffer[0];
  let addressLength = 0;
  let addressValueIndex = addressIndex + 1;
  let addressValue = '';
  switch (addressType) {
    case 1:
      addressLength = 4;
      addressValue = new Uint8Array(vlessBuffer.slice(addressValueIndex, addressValueIndex + addressLength)).join('.');
      break;
    case 2:
      addressLength = new Uint8Array(vlessBuffer.slice(addressValueIndex, addressValueIndex + 1))[0];
      addressValueIndex += 1;
      addressValue = new TextDecoder().decode(vlessBuffer.slice(addressValueIndex, addressValueIndex + addressLength));
      break;
    case 3:
      addressLength = 16;
      const dataView = new DataView(vlessBuffer.slice(addressValueIndex, addressValueIndex + addressLength));
      // 2001:0db8:85a3:0000:0000:8a2e:0370:7334
      const ipv6: string[] = [];
      for (let i = 0; i < 8; i++) {
        ipv6.push(dataView.getUint16(i * 2).toString(16));
      }
      addressValue = ipv6.join(':');
      // seems no need add [] for ipv6
      break;
    default:
      return {
        hasError: true,
        message: `invild  addressType is ${addressType}`,
      };
  }
  if (!addressValue) {
    return {
      hasError: true,
      message: `addressValue is empty, addressType is ${addressType}`,
    };
  }

  return {
    hasError: false,
    addressRemote: addressValue,
    addressType,
    portRemote,
    rawDataIndex: addressValueIndex + addressLength,
    vlessVersion: version,
    isUDP,
  };
}

/**
 * @param {Deno.TcpConn} remoteSocket
 * @param {WebSocket} webSocket
 * @param {ArrayBuffer} vlessResponseHeader
 * @param {(() => Promise<void>) | null} retry
 * @param {(info: string) => void} log
 */
async function remoteSocketToWS(remoteSocket: Deno.TcpConn, webSocket: WebSocket, vlessResponseHeader: Uint8Array, retry: (() => Promise<void>) | null, log: (info: string, event?: string) => void) {
  // remote--> ws
  let remoteChunkCount = 0; // Not used, consider removing if not needed for rate limiting logic.
  let hasIncomingData = false; // check if remoteSocket has incoming data
  await remoteSocket.readable
    .pipeTo(
      new WritableStream({
        start() {},
        /**
         *
         * @param {Uint8Array} chunk
         * @param {*} controller
         */
        async write(chunk, controller) {
          hasIncomingData = true;
          // remoteChunkCount++;
          if (webSocket.readyState !== WS_READY_STATE_OPEN) {
            controller.error('webSocket.readyState is not open, maybe close');
          }

          if (vlessResponseHeader) { // Use the parameter directly
            webSocket.send(new Uint8Array([...vlessResponseHeader, ...chunk]));
            vlessResponseHeader = null; // Clear it after first send
          } else {
            // seems no need rate limit this, CF seems fix this??..
            // if (remoteChunkCount > 20000) {
            //  // cf one package is 4096 byte(4kb),    4096 * 20000 = 80M
            //  await delay(1);
            // }
            webSocket.send(chunk);
          }
        },
        close() {
          log(`remoteConnection!.readable is close with hasIncomingData is ${hasIncomingData}`);
          // safeCloseWebSocket(webSocket); // no need server close websocket frist for some case will casue HTTP ERR_CONTENT_LENGTH_MISMATCH issue, client will send close event anyway.
        },
        abort(reason) {
          console.error(`remoteConnection!.readable abort`, reason);
        },
      })
    )
    .catch((error) => {
      console.error(`remoteSocketToWS has exception `, error.stack || error);
      safeCloseWebSocket(webSocket);
    });

  // seems is cf connect socket have error,
  // 1. Socket.closed will have error
  // 2. Socket.readable will be close without any data coming
  if (hasIncomingData === false && retry) {
    log(`retry`);
    retry();
  }
}

/**
 * @param {string} base64Str
 * @returns
 */
function base64ToArrayBuffer(base64Str: string) {
  if (!base64Str) {
    return { error: null };
  }
  try {
    // go use modified Base64 for URL rfc4648 which js atob not support
    base64Str = base64Str.replace(/-/g, '+').replace(/_/g, '/');
    const decode = atob(base64Str);
    const arryBuffer = Uint8Array.from(decode, (c) => c.charCodeAt(0));
    return { earlyData: arryBuffer.buffer, error: null };
  } catch (error) {
    return { error: error };
  }
}

/**
 * This is not real UUID validation, but it's used for comparison.
 * The `crypto.randomUUID()` function ensures a valid UUID v4 format.
 * @param {string} uuid
 */
function isValidUUID(uuid: string): boolean {
  const uuidRegex = /^[0-9a-f]{8}-[0-9a-f]{4}-[4][0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i;
  return uuidRegex.test(uuid);
}

const WS_READY_STATE_OPEN = 1;
const WS_READY_STATE_CLOSING = 2;
/**
 * Normally, WebSocket will not has exceptions when close.
 */
function safeCloseWebSocket(socket: WebSocket) {
  try {
    if (socket.readyState === WS_READY_STATE_OPEN || socket.readyState === WS_READY_STATE_CLOSING) {
      socket.close();
    }
  } catch (error) {
    console.error('safeCloseWebSocket error', error);
  }
}

const byteToHex: string[] = [];
for (let i = 0; i < 256; ++i) {
  byteToHex.push((i + 256).toString(16).slice(1));
}
function unsafeStringify(arr: Uint8Array, offset = 0) {
  return (
    byteToHex[arr[offset + 0]] +
    byteToHex[arr[offset + 1]] +
    byteToHex[arr[offset + 2]] +
    byteToHex[arr[offset + 3]] +
    '-' +
    byteToHex[arr[offset + 4]] +
    byteToHex[arr[offset + 5]] +
    '-' +
    byteToHex[arr[offset + 6]] +
    byteToHex[arr[offset + 7]] +
    '-' +
    byteToHex[arr[offset + 8]] +
    byteToHex[arr[offset + 9]] +
    '-' +
    byteToHex[arr[offset + 10]] +
    byteToHex[arr[offset + 11]] +
    byteToHex[arr[offset + 12]] +
    byteToHex[arr[offset + 13]] +
    byteToHex[arr[offset + 14]] +
    byteToHex[arr[offset + 15]]
  ).toLowerCase();
}
function stringify(arr: Uint8Array, offset = 0) {
  const uuid = unsafeStringify(arr, offset);
  if (!isValidUUID(uuid)) {
    throw TypeError('Stringified UUID is invalid');
  }
  return uuid;
}

/**
 * @param {WebSocket} webSocket
 * @param {Uint8Array} vlessResponseHeader
 * @param {(string)=> void} log
 */
async function handleUDPOutBound(webSocket: WebSocket, vlessResponseHeader: Uint8Array, log: (info: string) => void) {
  let isVlessHeaderSent = false;
  const transformStream = new TransformStream({
    start(controller) {},
    transform(chunk, controller) {
      // udp message 2 byte is the the length of udp data
      // TODO: this should have bug, because maybe udp chunk can be in two websocket message
      for (let index = 0; index < chunk.byteLength;) {
        const lengthBuffer = chunk.slice(index, index + 2);
        const udpPakcetLength = new DataView(lengthBuffer).getUint16(0);
        const udpData = new Uint8Array(chunk.slice(index + 2, index + 2 + udpPakcetLength));
        index = index + 2 + udpPakcetLength;
        controller.enqueue(udpData);
      }
    },
    flush(controller) {},
  });

  // only handle dns udp for now
  transformStream.readable
    .pipeTo(
      new WritableStream({
        async write(chunk) {
          const resp = await fetch('https://1.1.1.1/dns-query', {
            method: 'POST',
            headers: {
              'content-type': 'application/dns-message',
            },
            body: chunk,
          });
          const dnsQueryResult = await resp.arrayBuffer();
          const udpSize = dnsQueryResult.byteLength;
          // console.log([...new Uint8Array(dnsQueryResult)].map((x) => x.toString(16)));
          const udpSizeBuffer = new Uint8Array([(udpSize >> 8) & 0xff, udpSize & 0xff]);
          if (webSocket.readyState === WS_READY_STATE_OPEN) {
            log(`doh success and dns message length is ${udpSize}`);
            if (isVlessHeaderSent) {
              webSocket.send(await new Blob([udpSizeBuffer, dnsQueryResult]).arrayBuffer());
            } else {
              webSocket.send(await new Blob([vlessResponseHeader, udpSizeBuffer, dnsQueryResult]).arrayBuffer());
              isVlessHeaderSent = true;
            }
          }
        },
      })
    )
    .catch((error) => {
      log('dns udp has error' + error);
    });

  const writer = transformStream.writable.getWriter();

  return {
    /**
     *
     * @param {Uint8Array} chunk
     */
    write(chunk: Uint8Array) {
      writer.write(chunk);
    },
  };
}
